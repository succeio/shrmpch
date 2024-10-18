<script setup>
import { ref, provide, watch, onMounted, onUnmounted, nextTick } from 'vue'
import { useRoute, useRouter } from 'vue-router';
import { database } from './firebase'
import { ref as dbRef, onValue, query, get, orderByChild, limitToLast } from 'firebase/database'

import PostListTemplate from './components/postListTemplate.vue'
import PostTemplate from './components/postTemplate.vue'

import SendData from './components/sendData.vue'
import LineHeader from './components/lineHeader.vue'
import MainEnd from './components/mainEnd.vue';
import MiniHeader from './components/miniHeader.vue';
import NullPage from './components/nullPage.vue';

const posts = ref([])
const threads = ref([])
const postId = ref('')

const themeState = ref('')

//---------- router
const route = useRoute();
const router = useRouter();
//----------- router

const startPage = () => {
  posts.value = []
  threads.value = []
  themeState.value = ''
  router.push({ path: `/` })
}

let unsubscribe = null;

const fetchPosts = async () => {
  if (unsubscribe) {
    unsubscribe(); 
  }

  posts.value = [];
  threads.value = [];
  await nextTick();
  
  const postsRef = dbRef(database, `${route.params.board}/${route.params.thread}/posts`);

  
  unsubscribe = onValue(postsRef, (snapshot) => {
    const data = snapshot.val();

    if (data) {
      
      posts.value = Object.values(data); 

      
      if (posts.value.length > 0) {
        themeState.value = posts.value[0].theme; 
      } else {
        // Если постов нет, очищаем тему
        localStorage.removeItem('theme');
        themeState.value = '';
      }

    } else {
      posts.value = []; 
      localStorage.removeItem('theme'); 
      themeState.value = ''; 
    }
  });
};

const fetchThreads = async () => {
  if (unsubscribe) {
    unsubscribe(); 
  }

  themeState.value = ''
posts.value = []
  threads.value = []

  try {
    
    const sectionRef = query(
      dbRef(database, route.params.board),
      orderByChild('lastPostTimestamp'),
      limitToLast(10)
    )

    const snapshot = await get(sectionRef)

    if (snapshot.exists()) {
      const threadsData = snapshot.val()

      
      const threadPromises = Object.entries(threadsData).map(async ([threadKey, threadValue]) => {
        const opPost = threadValue.op; // Получаем данные "op" поста
        let posts = threadValue.posts ? Object.values(threadValue.posts) : []

        posts = posts.slice(-5).filter(post => post.postId !== opPost.postId)

        return {
          threadKey,
          op: opPost,
          posts, 
          lastPostTimestamp: threadValue.lastPostTimestamp 
        }
      })

      
      const loadedThreads = await Promise.all(threadPromises)

      
      threads.value = loadedThreads.sort((a, b) => b.lastPostTimestamp - a.lastPostTimestamp)
    }
  } catch (error) {
    console.error('Ошибка при загрузке тредов:', error)
  }

  
}

const getPostId = (id) => {
  if (postId.value === id) {
    postId.value = null; 
    setTimeout(() => {
      postId.value = id;
    });
  } else {
    postId.value = id;
  }

  if (route.params.thread) {
    window.scrollTo({
      top: document.body.scrollHeight,
      behavior: 'smooth'
    });
  }
};

provide('getPostId', getPostId)
provide('fetchPosts', fetchPosts)
provide('fetchThreads', fetchThreads)
provide('startPage', startPage)

const state = ref(!false)


watch(() => route.params, async (newParams) => {
  await nextTick(); 
  const { board, thread } = newParams;
  updateStateFromRoute(board, thread);
  document.title = board ? `🍤 shrmpch ${board}` : `🍤 shrmpch`
});


const checkSubnodeExistence = async (parentNode, subNode) => {
  const threadExist = dbRef(database, `${parentNode}/${subNode}`); 
  try {
    const snapshot = await get(threadExist);
    return snapshot.exists();
  } catch (error) {
    console.error("Ошибка проверки подузла:", error);
    return false;
  }
};

const updateStateFromRoute = async (board, thread) => {
  await nextTick()
  if (!board && !thread) {
    startPage()
  } else if (board && !thread) {
    fetchThreads()
  } else if (board && thread) {
    const exists = await checkSubnodeExistence(board, thread);
    if (exists) {
      console.log('OK');
    } else {
      startPage(); 
      return; 
    }
    fetchPosts()
  }
};


const newPostCount = ref(0) 


watch(posts, (newPosts, oldPosts) => {
  
  const oldPostsCount = oldPosts.length
  const newPostsCount = newPosts.length

  
  if (newPostsCount > oldPostsCount && document.hidden) {
    newPostCount.value += (newPostsCount - oldPostsCount) 
    document.title = `(${newPostCount.value}) Новые посты` 
  }
}, { deep: true }) 


const handleVisibilityChange = () => {
  if (!document.hidden) {
    newPostCount.value = 0 
    document.title = `🍤 shrmpch ${route.params.board}` 
  }
}


onMounted(() => {
  document.addEventListener('visibilitychange', handleVisibilityChange)
})


onUnmounted(() => {
  document.removeEventListener('visibilitychange', handleVisibilityChange)
})

</script>

<template>
  <MiniHeader />
  <div class="min-h-screen dark:bg-twitch bg-black">
    <LineHeader :themeState="themeState" />
    <div class="min-h-screen bg-white dark:bg-black rounded-t-2xl">
      <div class="pt-4">
        <button
          v-if="!route.params.thread && route.params.board"
          @click="
            () => {
              state = !state
            }
          "
          class="dark:bg-twitch ml-4 bg-black text-white rounded-2xl p-1 min-w-32"
        >
          {{ 'Создать тред' }}
        </button>
      </div>

      <div class="max-w-4xl">
        <SendData v-if="!state && !route.params.thread && route.params.board" :reply-id="postId" />
      </div>

      <NullPage v-if="!route.params.board" />

      <div class="ml-4">
        <PostListTemplate v-if="route.params.thread" :posts="posts" />

        <div v-auto-animate >
<div v-if="threads.length">
  
  <div
    v-for="(thread, threadIndex) in threads"
    :key="threadIndex"
  >
    <div class="pt-4">
      <!-- Выводим op-пост -->
      <PostTemplate
        :name="thread.op.name"
        :data="thread.op.data"
        :time="thread.op.time"
        :id=0
        :postId="thread.op.postId"
        :threadId="thread.op.threadId"
        :url="thread.op.url"
        :text="thread.op.text"
        :theme="thread.op.theme"
        :password="thread.op.password"
        :day="thread.op.day"
        :replies="thread.op.replies" 
      />

      <!-- Выводим последние 5 постов -->
      <div v-if="thread.posts.length">
        <PostTemplate
          v-for="(post, postIndex) in thread.posts"
          :key="postIndex"
          :name="post.name"
          :data="post.data"
          :time="post.time"
          :id=postIndex+1
          :postId="post.postId"
          :threadId="post.threadId"
          :url="post.url"
          :text="post.text"
          :theme="post.theme"
          :password="post.password"
          :day="post.day"
          :replies="post.replies" 
        />
      </div>
    </div>
  </div>
</div>

        </div>

        <SendData class="max-w-4xl" v-if="route.params.thread && posts.length > 0" :reply-id="postId" />
      </div>
    </div>
    
  </div>
  <MainEnd class="" />
</template>