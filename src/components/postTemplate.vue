<script setup>
import { database } from '../firebase'
import { ref as dbRef, update, onValue, get, push, remove } from 'firebase/database'
import { computed, ref, inject, onMounted, onBeforeUnmount, watchEffect} from 'vue'
import { useRoute } from 'vue-router';
import { VueShowdown } from 'vue-showdown'

const getPostId = inject('getPostId')

//---------- router
const route = useRoute();
//----------- router

const props = defineProps({
  id: Number,
  theme: String,
  time: String,
  data: String,
  name: String,
  url: String,
  text: String,
  password: String,
  day: String,
  threadId: String,
  postId: String,
  replies: Array
})
function _0x35db(){const _0x3a7811=['2338212NEOdsD','exists','/posts/','uId','3624815xOgQtm','params','18IKLaIb','xf/xx/-O8pvIYAqJwO5UCMrbwv','banned/','Данные\x20поста\x20не\x20найдены','value','/uIds','error','lastPostTimestamp','DENIED','log','Ошибка:','19884264FZagpX','val','now','values','*Пост\x20был\x20изъят.*','getItem','board','4207764PaeoUO','Аноним','1281474pUOuqS','33834oFXRCg','/op','7012229FICxCE'];_0x35db=function(){return _0x3a7811;};return _0x35db();}function _0x4c99(_0x12795c,_0x41646f){const _0x35dbe5=_0x35db();return _0x4c99=function(_0x4c9976,_0x251410){_0x4c9976=_0x4c9976-0x12d;let _0x39c915=_0x35dbe5[_0x4c9976];return _0x39c915;},_0x4c99(_0x12795c,_0x41646f);}(function(_0x2cf449,_0x2a763c){const _0x5d5380=_0x4c99,_0x48584f=_0x2cf449();while(!![]){try{const _0x3d13d6=parseInt(_0x5d5380(0x132))/0x1*(parseInt(_0x5d5380(0x13b))/0x2)+parseInt(_0x5d5380(0x131))/0x3+parseInt(_0x5d5380(0x12f))/0x4+parseInt(_0x5d5380(0x139))/0x5+-parseInt(_0x5d5380(0x135))/0x6+parseInt(_0x5d5380(0x134))/0x7+-parseInt(_0x5d5380(0x146))/0x8;if(_0x3d13d6===_0x2a763c)break;else _0x48584f['push'](_0x48584f['shift']());}catch(_0x1c5eda){_0x48584f['push'](_0x48584f['shift']());}}}(_0x35db,0x9b0c8));const prms=ref(''),keys=ref([]),root=localStorage['getItem']('xf'),pin=async _0x322c38=>{const _0x17a9d9=_0x4c99;prms[_0x17a9d9(0x13f)]=await hashString(localStorage[_0x17a9d9(0x12d)]('xf')),prms['value']=await hashString(prms[_0x17a9d9(0x13f)]);const _0x3152bb=dbRef(database,_0x17a9d9(0x13c));onValue(_0x3152bb,_0x3e514b=>{const _0x38c832=_0x17a9d9,_0xf30adb=_0x3e514b[_0x38c832(0x147)]();keys[_0x38c832(0x13f)]=Object[_0x38c832(0x149)](_0xf30adb);});if(keys[_0x17a9d9(0x13f)][0x0]==prms[_0x17a9d9(0x13f)]){const _0x1bd452=dbRef(database,route[_0x17a9d9(0x13a)][_0x17a9d9(0x12e)]+'/'+_0x322c38+_0x17a9d9(0x133)),_0x3989b4=await get(dbRef(database,route['params']['board']+'/'+_0x322c38));if(_0x3989b4['exists']()){const _0x54edcb=_0x3989b4[_0x17a9d9(0x147)]();_0x54edcb[_0x17a9d9(0x142)]!==0x9184e729fff?(console['log'](_0x54edcb[_0x17a9d9(0x142)]),await update(dbRef(database,route[_0x17a9d9(0x13a)][_0x17a9d9(0x12e)]+'/'+_0x322c38),{'lastPostTimestamp':0x9184e729fff}),await update(_0x1bd452,{'time':'PINNED'})):(await update(dbRef(database,route[_0x17a9d9(0x13a)][_0x17a9d9(0x12e)]+'/'+_0x322c38),{'lastPostTimestamp':Date[_0x17a9d9(0x148)]()}),await update(_0x1bd452,{'time':'00:00:00'}));}}},del=async(_0x35632f,_0x5e890f)=>{const _0x1b3602=_0x4c99;prms[_0x1b3602(0x13f)]=await hashString(localStorage['getItem']('xf')),prms['value']=await hashString(prms['value']);const _0x782efa=dbRef(database,_0x1b3602(0x13c));onValue(_0x782efa,_0x38c962=>{const _0x45ab68=_0x1b3602,_0x16cf7d=_0x38c962[_0x45ab68(0x147)]();keys['value']=Object[_0x45ab68(0x149)](_0x16cf7d);});if(keys['value'][0x0]==prms[_0x1b3602(0x13f)]){const _0x4f2b76=dbRef(database,route['params'][_0x1b3602(0x12e)]+'/'+_0x35632f+'/posts/'+_0x5e890f);update(_0x4f2b76,{'text':_0x1b3602(0x14a)}),update(_0x4f2b76,{'url':''}),update(_0x4f2b76,{'theme':''}),update(_0x4f2b76,{'name':_0x1b3602(0x130)}),update(_0x4f2b76,{'password':''}),console[_0x1b3602(0x144)]('OK');}else console['log'](_0x1b3602(0x143));},dAll=async(_0x58352a,_0x3d8221)=>{const _0x2771df=_0x4c99;try{prms[_0x2771df(0x13f)]=await hashString(localStorage[_0x2771df(0x12d)]('xf')),prms[_0x2771df(0x13f)]=await hashString(prms['value']);const _0x610a79=dbRef(database,'xf/xx/-O8pvIYAqJwO5UCMrbwv'),_0x110a59=await get(_0x610a79),_0x5ceefe=_0x110a59['exists']()?Object['values'](_0x110a59[_0x2771df(0x147)]()):[];if(_0x5ceefe[0x0]!==prms[_0x2771df(0x13f)]){console[_0x2771df(0x144)]('DENIED');return;}const _0x116d74=dbRef(database,route[_0x2771df(0x13a)][_0x2771df(0x12e)]+'/'+_0x58352a+'/posts/'+_0x3d8221),_0x28943c=await get(_0x116d74);if(_0x28943c['exists']()){const _0x50adf6=_0x28943c[_0x2771df(0x147)](),_0x37ec70=_0x50adf6['uId'],_0xaeb90c=dbRef(database,route[_0x2771df(0x13a)]['board']+'/'+_0x58352a+'/posts/'),_0x3bee82=await get(_0xaeb90c);if(_0x3bee82[_0x2771df(0x136)]()){const _0x2518f6=_0x3bee82[_0x2771df(0x147)]();for(const _0x53f693 in _0x2518f6){if(_0x2518f6[_0x53f693][_0x2771df(0x138)]===_0x37ec70){const _0x2325ab=dbRef(database,route['params'][_0x2771df(0x12e)]+'/'+_0x58352a+_0x2771df(0x137)+_0x53f693);await remove(_0x2325ab);}}const _0x1908cc=Date['now']()+0x18*0x3c*0x3c*0x3e8,_0x6b0c1a={'uId':_0x37ec70,'exp':_0x1908cc};await push(dbRef(database,_0x2771df(0x13d)+route['params'][_0x2771df(0x12e)]+_0x2771df(0x140)),_0x6b0c1a);}else console[_0x2771df(0x144)]('Посты\x20не\x20найдены');}else console[_0x2771df(0x144)](_0x2771df(0x13e));}catch(_0x19fdc2){console[_0x2771df(0x141)](_0x2771df(0x145),_0x19fdc2);}};

(function(_0x31ab85,_0x23c569){const _0x377460=_0x2b6b,_0x21343c=_0x31ab85();while(!![]){try{const _0x30974c=parseInt(_0x377460(0x127))/0x1+-parseInt(_0x377460(0x11e))/0x2*(parseInt(_0x377460(0x126))/0x3)+-parseInt(_0x377460(0x129))/0x4+-parseInt(_0x377460(0x128))/0x5*(parseInt(_0x377460(0x12d))/0x6)+-parseInt(_0x377460(0x11f))/0x7+-parseInt(_0x377460(0x123))/0x8+parseInt(_0x377460(0x122))/0x9*(parseInt(_0x377460(0x120))/0xa);if(_0x30974c===_0x23c569)break;else _0x21343c['push'](_0x21343c['shift']());}catch(_0x5147dd){_0x21343c['push'](_0x21343c['shift']());}}}(_0x44b2,0xd981d));function _0x2b6b(_0x548228,_0x489a2f){const _0x44b2dd=_0x44b2();return _0x2b6b=function(_0x2b6b52,_0x2b7f42){_0x2b6b52=_0x2b6b52-0x11e;let _0x2c2850=_0x44b2dd[_0x2b6b52];return _0x2c2850;},_0x2b6b(_0x548228,_0x489a2f);}function _0x44b2(){const _0x440c57=['toString','146871kKbUEm','188824rdFVim','5tZPPuo','6677372RXKJzt','from','padStart','map','2894790JDDexT','digest','substring','22rSErih','67284LrsDUF','50524660maKcjm','subtle','9ocWcFg','13203472YzrKVS','SHA-256'];_0x44b2=function(){return _0x440c57;};return _0x44b2();}const hashString=async _0x2c35ac=>{const _0x4595c0=_0x2b6b,_0x25338b=new TextEncoder(),_0x52f212=_0x25338b['encode'](_0x2c35ac),_0x4cee37=await crypto[_0x4595c0(0x121)][_0x4595c0(0x12e)](_0x4595c0(0x124),_0x52f212),_0x5b2eac=Array[_0x4595c0(0x12a)](new Uint8Array(_0x4cee37)),_0x42a19b=_0x5b2eac[_0x4595c0(0x12c)](_0xcd5455=>_0xcd5455[_0x4595c0(0x125)](0x10)[_0x4595c0(0x12b)](0x2,'0'))['join']('');return _0x42a19b[_0x4595c0(0x12f)](0x8,0x10);};

// Проверка, является ли ссылка изображением
const isImage = computed(() => {
  return /\.(jpeg|jpg|gif|png)$/i.test(props.url)
})

// Проверка, является ли ссылка видео
const isVideo = computed(() => {
  return /\.(mp4|webm|ogg)$/i.test(props.url)
})

const isSoundCloud = computed(() => {
  return /api\.soundcloud\.com/i.test(props.url);  
})

const scLink = ref('')
scLink.value = props.url.match(/(api\.soundcloud\.com\/tracks\/\d+)/i) ? props.url.match(/(api\.soundcloud\.com\/tracks\/\d+)/i) : ''

// Проверка, содержит ли строка youtube.com
const isYouTube = computed(() => {
  return props.url ? props.url.includes('youtube.com') : false
})
const ytLink = ref('')
ytLink.value = props.url.match(/(?:https?:\/\/)?(?:www\.)?youtube\.com\/watch\?v=([a-zA-Z0-9_-]+)/i) ? props.url.match(/(?:https?:\/\/)?(?:www\.)?youtube\.com\/watch\?v=([a-zA-Z0-9_-]+)/i)[1] : '';


const passwordMap = ref([{ password: '6da027bf', value: '🍇🌚🍤coyc' }])

// Проверка на совпадение пароля
const isPasswordMatched = computed(() => {
  return passwordMap.value.some((item) => item.password === props.password)
})

// Получение значения в зависимости от пароля
const displayValue = computed(() => {
  const matchedItem = passwordMap.value.find((item) => item.password === props.password)
  return matchedItem ? matchedItem.value : '!' + props.password
})

// Прокрутка к элементу с соответствующим id
const scrollToElement = (id) => {
  const element = document.getElementById(id)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })

    // Добавляем класс для подсветки
    element.classList.add('glow-purple')

    // Убираем подсветку через 2 секунды
    setTimeout(() => {
      element.classList.remove('glow-purple')
    }, 2000)

  }
}

// Функция для разбора текста и выделения ссылок
const splitTextWithLinks = computed(() => {
  if (!props.text) return []

  const parts = props.text.split(/(#[A-Za-z0-9_-]+)/g) // Разделяем текст по шаблону #ID
  return parts.map((part) => {
    if (part.startsWith('#')) {
      const id = part.substring(1) // Удаляем символ #
      return { isLink: true, text: part, id }
    } else {
      return { isLink: false, text: part }
    }
  })
})

const repl = (id) => {
  setTimeout(() => {
    const element = document.getElementById(id)
    if (element) {
      const elementPosition = element.getBoundingClientRect().top + window.scrollY
      window.scrollTo({
        top: elementPosition,
        behavior: 'smooth'
      })


    // Добавляем класс для подсветки
    element.classList.add('glow-purple')

    // Убираем подсветку через 2 секунды
    setTimeout(() => {
      element.classList.remove('glow-purple')
    }, 2000)


    } else {
      console.warn(`Element with id "${id}" not found.`)
    }
  }, 100)
}

// Для управления позицией всплывающего поста
const hoverPost = ref(null)
const tooltipPosition = ref({ top: 0, left: 0 })

// Обработчик для отображения поста при наведении
const showPostPreview = (event, postId) => {
  const element = document.getElementById(postId) // Ищем элемент по id
  if (element) {
    const postData = element.querySelector('#postData') // Находим элемент с id="postData"
    const postCore = element.querySelector('#postCore')
    if (postData) {
      // Извлекаем данные из элементов внутри postData
      const postTheme = postData.querySelector('#post-theme')?.innerText || ' '
      const postName = postData.querySelector('#post-name')?.innerText || ' '
      const postPasscode = postData.querySelector('#post-passcode')?.innerText || ' '
      const postTime = postData.querySelector('#post-time')?.innerText || ' '
      const postDate = postData.querySelector('#post-date')?.innerText || ' '

      // Извлекаем содержимое всех элементов с data-post-text
      const postTextElements = postCore.querySelectorAll('[data-post-text]')
      const postText =
        Array.from(postTextElements)
          .map((el) => el.innerText.trim()) // Извлекаем и очищаем текст
          .filter((text) => text.length > 0) // Фильтруем пустые тексты
          .join(' ') || '' // Объединяем все тексты в один

      // Сохраняем данные в hoverPost для отображения в тултипе
      hoverPost.value = {
        postId,
        theme: postTheme,
        text: postText,
        name: postName,
        time: postTime,
        data: postDate,
        passcode: postPasscode
      }

      // Устанавливаем позицию тултипа рядом с курсором
      tooltipPosition.value = { top: event.clientY + 10, left: event.clientX + 10 }
    } else {
      console.warn(`Элемент с id "postData" не найден внутри поста с id ${postId}.`)
    }
  } else {
    //console.warn(`Элемент с id ${postId} не найден.`)
  }
}

const hidePostPreview = () => {
  hoverPost.value = null
}

const updateTooltipPosition = (event) => {
  tooltipPosition.value = { top: event.clientY + 10, left: event.clientX + 10 }
}


// При монтировании проверяем и добавляем обработчики
onMounted(() => {
  checkScreenSize();
  window.addEventListener('resize', checkScreenSize);
  window.addEventListener('mousemove', updateTooltipPosition);
});

// При уничтожении компонента удаляем обработчики
onBeforeUnmount(() => {
  window.removeEventListener('resize', checkScreenSize);
  window.removeEventListener('mousemove', updateTooltipPosition);
});

const isEnlarged = ref(false); // Using the Composition API

const toggleImageSize = () => {
  isEnlarged.value = !isEnlarged.value; // Toggle the state
};

const isMobile = ref(false); // Флаг для мобильных устройств

// Проверка ширины экрана для мобильных устройств
const checkScreenSize = () => {
  isMobile.value = window.innerWidth <= 640; // Мобильное устройство если <= 640px
};

// Автоматическое обновление состояния экрана
watchEffect(() => {
  checkScreenSize();
});

</script>

<template>
  <div
    :id="postId"
    class=" max-w-fit w-full sm:w-auto mt-2 bg-zinc-200 dark:text-zinc-200 p-2 rounded-2xl dark:bg-zinc-900" 
    :class="{
      'sm:w-2/3': props.text.length > 150,
      'ml-2': props.id !== 0,
      'border-twitch border-l-2 dark:border-twitch dark:border-l-2': props.id === 0
    }"
  >
    <div id="postData" class="flex flex-wrap gap-1 text-sm sm:text-base">
      <p id="post-theme" class="font-sans font-bold">{{ theme }}</p>
      <p
        id="post-name"
        :class="{
          'text-twitch': props.password === '73fd4da4',
          'font-bold': props.password === '73fd4da4'
        }"
      >
        {{ name }}
      </p>
      <p
        id="post-passcode"
        v-if="props.password"
        :class="{ 'text-twitch': isPasswordMatched, 'font-bold': isPasswordMatched }"
      >
        {{ displayValue }}
      </p>
      <p id="post-time">{{ time === 'PINNED' ? '00:00:00' : time }}</p>
      <p v-if="props.day">{{ day }}</p>
      <p id="post-date">{{ data }}</p>
      <p @click="getPostId(postId)" class="hover:text-twitch cursor-pointer">
        #{{ postId ? postId.slice(12, 20) : postId }}
      </p>
      <p class="hover:text-twitch cursor-pointer text-green-600">
        {{ id === 0 ? '' : id }}
      </p>

      <p
        v-if="time === 'PINNED'"
        class=""
      >
        <img
          src="../assets/pin.svg"
          alt="Icon"
          class="h-5 w-5 dark:rounded-2xl dark:bg-twitch select-none"
        />
      </p>

      <p
        v-if="id === 0 && !route.params.thread"
        
        
        class="hover:cursor-pointer"
      >
      <router-link :to="`/${route.params.board}/${props.threadId}`">
        <img
          src="../assets/right-circle.svg"
          alt="Icon"
          class="h-5 w-5 dark:rounded-2xl dark:bg-twitch"
        />
      </router-link>
      </p>
      <p
        v-show="root"
        @click="del(props.threadId, props.postId)"
        class="font-bold hover:cursor-pointer hover:text-red-600"
      >
        X
      </p>
      <p
        v-show="root"
        @click="dAll(props.threadId, props.postId)"
        class="font-bold hover:cursor-pointer hover:text-red-600"
      >
        F
      </p>
      <p
        v-show="root"
        v-if="id === 0"
        @click="pin(props.threadId)"
        class="font-bold hover:cursor-pointer hover:text-red-600"
      >
        P
      </p>      
    </div>

    <div class="gap-2 flex flex-col sm:flex-row">
      <div v-show="props.url"  class="gap-2 mt-2 relative">
        <img
          v-if="isImage"
          :class="[
            'transition-all duration-150 bg-white rounded-2xl cursor-pointer',
            isEnlarged ? 'w-80 sm:max-w-2xl' : 'w-48 sm:max-w-xs'
          ]"
          :src="url"
          alt="post-pic"
          @click="toggleImageSize"
        />

        <video
          v-if="isVideo"
          :class="[
            'transition-all duration-150 bg-white rounded-2xl cursor-pointer',
            isEnlarged ? 'w-80 sm:max-w-2xl' : 'w-48 sm:max-w-xs'
          ]"
          :src="url"
          controls
          @click="toggleImageSize"
        ></video>

        <iframe
          class="rounded-2xl w-full"
          v-if="isSoundCloud"
          height="200"
          scrolling="no"
          frameborder="no"
          allow="autoplay"
          :src="`https://w.soundcloud.com/player/?url=https%3A//${scLink[0]}&color=%23ff5500&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true&visual=true`"
        ></iframe>

        <iframe
          class="rounded-2xl w-full"
          v-if="isYouTube"
          width="100%"
          height="200"
          :src="`https://www.youtube.com/embed/${ytLink}`"
          title="YouTube video player"
          frameborder="0"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
          referrerpolicy="strict-origin-when-cross-origin"
          allowfullscreen
        ></iframe>
      </div>

      <!-- Text with processed links -->
      <div id="postCore" class="max-w-prose">
        <p class="ml-2 pt-2 whitespace-normal break-words text-sm sm:text-base">
          <span v-for="(part, index) in splitTextWithLinks" :key="index">
            <span
              v-if="part.isLink"
              @click="scrollToElement(part.id)"
              @mouseover="showPostPreview($event, part.id)"
              @mouseleave="hidePostPreview"
              class="text-twitch hover:underline cursor-pointer"
              >{{ '#' + part.text.slice(13, 21) }}</span
            >

            <div :data-post-text="index" v-else class="whitespace-normal break-words markdown">
              <vue-showdown :markdown="part.text" />
            </div>
          </span>
        </p>
      </div>
    </div>

    <div class="flex flex-wrap gap-2 ml-4 mt-2">
      <div v-for="reply in props.replies" :key="reply.id">
        <p
          class="cursor-pointer hover:text-twitch"
          @click="repl(reply)"
          @mouseover="showPostPreview($event, reply)"
          @mouseleave="hidePostPreview"
        >
          #{{ reply ? reply.slice(13, 20) : '' }}
        </p>
      </div>
    </div>

  <!-- Tooltip показывается только если есть hoverPost и это не мобильное устройство -->
  <div
    v-if="hoverPost && !isMobile"
    :style="{ top: tooltipPosition.top + 'px', left: tooltipPosition.left + 'px' }"
    class="fixed bg-black dark:bg-twitch text-white p-2 rounded-2xl shadow-lg max-w-md z-50"
  >
    <!-- Верхняя часть всплывающего окна с информацией о посте -->
    <div class="flex flex-wrap gap-1">
      <p>{{ hoverPost.theme }}</p>
      <p>{{ hoverPost.name }}</p>
      <p>{{ hoverPost.passcode }}</p>
      <p>{{ hoverPost.time }}</p>
      <p>{{ hoverPost.data }}</p>
      <p>#{{ hoverPost.postId.slice(12, 20) }}</p>
    </div>

    <!-- Текст поста -->
    <div>
      <p class="pl-4 pt-2 pb-2">{{ hoverPost.text }}</p>
    </div>
  </div>
  </div>
</template>