<script setup>
import { database } from '../firebase'
import { ref as dbRef, update, onValue, get, push, remove } from 'firebase/database'
import { computed, ref, inject, onMounted, onBeforeUnmount } from 'vue'
import { VueShowdown } from 'vue-showdown'

const fetchPosts = inject('fetchPosts')
const getPostId = inject('getPostId')

const boardState = ref('')

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

//function _0x4080(_0x5193b7,_0x19d353){const _0x179cdf=_0x179c();return _0x4080=function(_0x40808d,_0x4d0766){_0x40808d=_0x40808d-0xa5;let _0x18b6b2=_0x179cdf[_0x40808d];return _0x18b6b2;},_0x4080(_0x5193b7,_0x19d353);}function _0x179c(){const _0x379955=['1233iecGcr','boardState','5223936AAbUIb','1210367Zntkvh','getItem','values','1166040JYQfaB','Аноним','value','84SvqPAH','/posts/','*Пост\x20был\x20изъят.*','246504gCzrNe','log','6313475nubLnL','578599nUdHsS','2598936eozdWz'];_0x179c=function(){return _0x379955;};return _0x179c();}const _0x2bcd1e=_0x4080;(function(_0x514610,_0xbbd4cf){const _0x5dbb3d=_0x4080,_0x547ca6=_0x514610();while(!![]){try{const _0x3600c8=-parseInt(_0x5dbb3d(0xad))/0x1+-parseInt(_0x5dbb3d(0xa9))/0x2+parseInt(_0x5dbb3d(0xb0))/0x3+-parseInt(_0x5dbb3d(0xac))/0x4+-parseInt(_0x5dbb3d(0xa7))/0x5+-parseInt(_0x5dbb3d(0xb3))/0x6*(-parseInt(_0x5dbb3d(0xa8))/0x7)+parseInt(_0x5dbb3d(0xa5))/0x8*(parseInt(_0x5dbb3d(0xaa))/0x9);if(_0x3600c8===_0xbbd4cf)break;else _0x547ca6['push'](_0x547ca6['shift']());}catch(_0x58e245){_0x547ca6['push'](_0x547ca6['shift']());}}}(_0x179c,0xa8269));const prms=ref(''),keys=ref([]),root=localStorage[_0x2bcd1e(0xae)]('xf'),del=async(_0x2d64a5,_0x24383a)=>{const _0x36e0a1=_0x2bcd1e;boardState[_0x36e0a1(0xb2)]=localStorage[_0x36e0a1(0xae)](_0x36e0a1(0xab)),prms[_0x36e0a1(0xb2)]=await hashString(localStorage['getItem']('xf')),prms[_0x36e0a1(0xb2)]=await hashString(prms[_0x36e0a1(0xb2)]);const _0x32f175=dbRef(database,'xf/xx/-O8pvIYAqJwO5UCMrbwv');onValue(_0x32f175,_0x49a084=>{const _0x53edf7=_0x36e0a1,_0xa78bf2=_0x49a084['val']();keys[_0x53edf7(0xb2)]=Object[_0x53edf7(0xaf)](_0xa78bf2);});if(keys[_0x36e0a1(0xb2)][0x0]==prms[_0x36e0a1(0xb2)]){const _0x4923f7=dbRef(database,boardState[_0x36e0a1(0xb2)]+'/'+_0x2d64a5+_0x36e0a1(0xb4)+_0x24383a);update(_0x4923f7,{'text':_0x36e0a1(0xb5)}),update(_0x4923f7,{'url':''}),update(_0x4923f7,{'theme':''}),update(_0x4923f7,{'name':_0x36e0a1(0xb1)}),update(_0x4923f7,{'password':''}),console[_0x36e0a1(0xa6)]('OK');}else console['log']('DENIED');};

const _0x4bdef1=_0x12ca;(function(_0x1155e8,_0x27bfd9){const _0x24923e=_0x12ca,_0x5241c1=_0x1155e8();while(!![]){try{const _0x3b4458=parseInt(_0x24923e(0x1b2))/0x1*(parseInt(_0x24923e(0x1ae))/0x2)+parseInt(_0x24923e(0x1a4))/0x3+-parseInt(_0x24923e(0x19d))/0x4*(-parseInt(_0x24923e(0x1a0))/0x5)+parseInt(_0x24923e(0x197))/0x6*(parseInt(_0x24923e(0x19a))/0x7)+-parseInt(_0x24923e(0x1a8))/0x8+parseInt(_0x24923e(0x1a9))/0x9*(parseInt(_0x24923e(0x1a6))/0xa)+-parseInt(_0x24923e(0x1b3))/0xb*(parseInt(_0x24923e(0x1a1))/0xc);if(_0x3b4458===_0x27bfd9)break;else _0x5241c1['push'](_0x5241c1['shift']());}catch(_0x5a4027){_0x5241c1['push'](_0x5241c1['shift']());}}}(_0x23c5,0x27960));function _0x12ca(_0x178e71,_0x36d45c){const _0x23c5bb=_0x23c5();return _0x12ca=function(_0x12ca87,_0x320b4b){_0x12ca87=_0x12ca87-0x196;let _0x8ddefb=_0x23c5bb[_0x12ca87];return _0x8ddefb;},_0x12ca(_0x178e71,_0x36d45c);}const prms=ref(''),keys=ref([]),root=localStorage[_0x4bdef1(0x1ac)]('xf'),del=async(_0x51f347,_0x381e70)=>{const _0x3f3a2d=_0x4bdef1;boardState[_0x3f3a2d(0x19e)]=localStorage[_0x3f3a2d(0x1ac)]('boardState'),prms[_0x3f3a2d(0x19e)]=await hashString(localStorage[_0x3f3a2d(0x1ac)]('xf')),prms[_0x3f3a2d(0x19e)]=await hashString(prms[_0x3f3a2d(0x19e)]);const _0x1fafab=dbRef(database,_0x3f3a2d(0x1a7));onValue(_0x1fafab,_0x199fc9=>{const _0x1bcf01=_0x3f3a2d,_0x5aac57=_0x199fc9[_0x1bcf01(0x198)]();keys[_0x1bcf01(0x19e)]=Object[_0x1bcf01(0x1a3)](_0x5aac57);});if(keys[_0x3f3a2d(0x19e)][0x0]==prms[_0x3f3a2d(0x19e)]){const _0x1dce47=dbRef(database,boardState[_0x3f3a2d(0x19e)]+'/'+_0x51f347+'/posts/'+_0x381e70);update(_0x1dce47,{'text':_0x3f3a2d(0x19b)}),update(_0x1dce47,{'url':''}),update(_0x1dce47,{'theme':''}),update(_0x1dce47,{'name':_0x3f3a2d(0x19c)}),update(_0x1dce47,{'password':''}),console[_0x3f3a2d(0x1b0)]('OK');}else console[_0x3f3a2d(0x1b0)](_0x3f3a2d(0x19f));},dAll=async(_0x385697,_0x2606e1)=>{const _0x42d488=_0x4bdef1;try{boardState[_0x42d488(0x19e)]=localStorage[_0x42d488(0x1ac)](_0x42d488(0x1a2)),prms[_0x42d488(0x19e)]=await hashString(localStorage['getItem']('xf')),prms[_0x42d488(0x19e)]=await hashString(prms['value']);const _0x28c505=dbRef(database,_0x42d488(0x1a7)),_0x20c85c=await get(_0x28c505),_0x1f79aa=_0x20c85c[_0x42d488(0x199)]()?Object[_0x42d488(0x1a3)](_0x20c85c['val']()):[];if(_0x1f79aa[0x0]!==prms['value']){console['log'](_0x42d488(0x19f));return;}const _0x468e3e=dbRef(database,boardState[_0x42d488(0x19e)]+'/'+_0x385697+'/posts/'+_0x2606e1),_0x2bdbe9=await get(_0x468e3e);if(_0x2bdbe9[_0x42d488(0x199)]()){const _0x5d1d97=_0x2bdbe9[_0x42d488(0x198)](),_0x2e8b09=_0x5d1d97[_0x42d488(0x1a5)],_0x2c6d2b=dbRef(database,boardState['value']+'/'+_0x385697+_0x42d488(0x1b1)),_0x50a68a=await get(_0x2c6d2b);if(_0x50a68a[_0x42d488(0x199)]()){const _0x26aeb2=_0x50a68a['val']();for(const _0x1847c8 in _0x26aeb2){if(_0x26aeb2[_0x1847c8][_0x42d488(0x1a5)]===_0x2e8b09){const _0x93c0da=dbRef(database,boardState[_0x42d488(0x19e)]+'/'+_0x385697+_0x42d488(0x1b1)+_0x1847c8);await remove(_0x93c0da);}}const _0x12b44e=Date[_0x42d488(0x1ab)]()+0x18*0x3c*0x3c*0x3e8,_0x826d09={'uId':_0x2e8b09,'exp':_0x12b44e};await push(dbRef(database,_0x42d488(0x1ad)+boardState[_0x42d488(0x19e)]+_0x42d488(0x196)),_0x826d09);}else console[_0x42d488(0x1b0)]('Посты\x20не\x20найдены');}else console[_0x42d488(0x1b0)](_0x42d488(0x1af));}catch(_0xb57cb1){console[_0x42d488(0x1aa)]('Ошибка:',_0xb57cb1);}};function _0x23c5(){const _0x4036e1=['getItem','banned/','428046CjIjRn','Данные\x20поста\x20не\x20найдены','log','/posts/','1tmPhnu','66CSlHzT','/uIds','30mqplgK','val','exists','350777uDsLUH','*Пост\x20был\x20изъят.*','Аноним','1221032wVYolT','value','DENIED','5BYIcwu','964380YvjVOW','boardState','values','330642RtsxYe','uId','1070JBUpjA','xf/xx/-O8pvIYAqJwO5UCMrbwv','2277776rKnOuE','4122wBaUwg','error','now'];_0x23c5=function(){return _0x4036e1;};return _0x23c5();}

function _0x2a5d(_0x30e1d7,_0x28be83){const _0x1242a5=_0x1242();return _0x2a5d=function(_0x2a5d5f,_0x3b7d78){_0x2a5d5f=_0x2a5d5f-0xb6;let _0x511e22=_0x1242a5[_0x2a5d5f];return _0x511e22;},_0x2a5d(_0x30e1d7,_0x28be83);}function _0x1242(){const _0x5cbe22=['substring','282464cCbLau','4lWqTFH','649994piRjmw','7VTObGZ','109813hpWpHe','3375427mRCltr','10933584GVrswh','join','20AqbhON','801342XOAPOF','digest','144zKcbJc','259340mbEgMY','subtle','SHA-256','map','3ORQgKF','from'];_0x1242=function(){return _0x5cbe22;};return _0x1242();}(function(_0x3ea490,_0x22fbfc){const _0x23797b=_0x2a5d,_0x1329a2=_0x3ea490();while(!![]){try{const _0x18ffc8=-parseInt(_0x23797b(0xc0))/0x1+parseInt(_0x23797b(0xbe))/0x2*(parseInt(_0x23797b(0xb9))/0x3)+parseInt(_0x23797b(0xbd))/0x4*(-parseInt(_0x23797b(0xc8))/0x5)+parseInt(_0x23797b(0xc5))/0x6*(parseInt(_0x23797b(0xbf))/0x7)+parseInt(_0x23797b(0xbc))/0x8*(parseInt(_0x23797b(0xc7))/0x9)+-parseInt(_0x23797b(0xc4))/0xa*(-parseInt(_0x23797b(0xc1))/0xb)+-parseInt(_0x23797b(0xc2))/0xc;if(_0x18ffc8===_0x22fbfc)break;else _0x1329a2['push'](_0x1329a2['shift']());}catch(_0x465458){_0x1329a2['push'](_0x1329a2['shift']());}}}(_0x1242,0x89c9f));const hashString=async _0x4e650a=>{const _0x3c7b3a=_0x2a5d,_0x5af142=new TextEncoder(),_0x2f51ce=_0x5af142['encode'](_0x4e650a),_0x20c316=await crypto[_0x3c7b3a(0xb6)][_0x3c7b3a(0xc6)](_0x3c7b3a(0xb7),_0x2f51ce),_0x285964=Array[_0x3c7b3a(0xba)](new Uint8Array(_0x20c316)),_0x5b8274=_0x285964[_0x3c7b3a(0xb8)](_0x1e15d1=>_0x1e15d1['toString'](0x10)['padStart'](0x2,'0'))[_0x3c7b3a(0xc3)]('');return _0x5b8274[_0x3c7b3a(0xbb)](0x8,0x10);};

const openThread = (thread) => {
  localStorage.setItem('threadState', thread)
  fetchPosts()
}

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

function _0x7600(){const _0x4be611=['6zAuWuS','589222ENwHIt','1742797uKPOuT','6da027bf','value','961870waJDiY','password','298296CiIkjV','968011JqoWaS','160BLhPkK','12lYyQkr','9HUYCrZ','4652216QfTDyi','423696icVbnM'];_0x7600=function(){return _0x4be611;};return _0x7600();}const _0x1e0dfc=_0x1212;function _0x1212(_0x505d10,_0x516cf2){const _0x760064=_0x7600();return _0x1212=function(_0x1212c8,_0x371db2){_0x1212c8=_0x1212c8-0x110;let _0x3fc140=_0x760064[_0x1212c8];return _0x3fc140;},_0x1212(_0x505d10,_0x516cf2);}(function(_0x199cdb,_0x59f3b9){const _0x29a4a8=_0x1212,_0x34b313=_0x199cdb();while(!![]){try{const _0x24fa27=-parseInt(_0x29a4a8(0x118))/0x1+-parseInt(_0x29a4a8(0x116))/0x2+parseInt(_0x29a4a8(0x110))/0x3*(parseInt(_0x29a4a8(0x113))/0x4)+-parseInt(_0x29a4a8(0x11c))/0x5+-parseInt(_0x29a4a8(0x117))/0x6*(-parseInt(_0x29a4a8(0x119))/0x7)+-parseInt(_0x29a4a8(0x115))/0x8*(parseInt(_0x29a4a8(0x114))/0x9)+parseInt(_0x29a4a8(0x112))/0xa*(parseInt(_0x29a4a8(0x111))/0xb);if(_0x24fa27===_0x59f3b9)break;else _0x34b313['push'](_0x34b313['shift']());}catch(_0x528f58){_0x34b313['push'](_0x34b313['shift']());}}}(_0x7600,0x5cd98));const passwordMap=ref([{'password':_0x1e0dfc(0x11a),'value':'🍇🌚🍤coyc'}]),isPasswordMatched=computed(()=>{const _0x5205a1=_0x1e0dfc;return passwordMap[_0x5205a1(0x11b)]['some'](_0x395a7a=>_0x395a7a[_0x5205a1(0x11d)]===props['password']);});

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
onBeforeUnmount
// Обновляем позицию тултипа при движении мыши
onMounted(() => {
  window.addEventListener('mousemove', updateTooltipPosition)
})

onBeforeUnmount(() => {
  window.removeEventListener('mousemove', updateTooltipPosition)
})

const isEnlarged = ref(false); // Using the Composition API

const toggleImageSize = () => {
  isEnlarged.value = !isEnlarged.value; // Toggle the state
};

</script>

<template>
  <div
    :id="postId"
    class="max-w-fit w-full sm:w-auto mt-2 bg-zinc-200 dark:text-white p-2 rounded-2xl dark:bg-zinc-900"
    :class="{
      'sm:w-2/3': props.text.length > 150,
      'ml-2': props.id !== 0,
      'border-twitch border-l-2 dark:border-twitch dark:border-l-2': props.id === 0
    }"
  >
    <div id="postData" class="flex flex-wrap gap-2 text-sm sm:text-base">
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
      <p id="post-time">{{ time }}</p>
      <p v-if="props.day">{{ day }}</p>
      <p id="post-date">{{ data }}</p>
      <p @click="getPostId(postId)" class="hover:text-twitch cursor-pointer">
        #{{ postId ? postId.slice(12, 20) : postId }}
      </p>
      <p class="hover:text-twitch cursor-pointer text-green-600">
        {{ id === 0 ? '0P' : id }}
      </p>
      <p
        v-if="id === 0"
        @keyup.ctrl.left="openThread(threadId)"
        @click="openThread(threadId)"
        class="hover:cursor-pointer"
      >
        <img
          src="../assets/right-circle.svg"
          alt="Icon"
          class="h-4 w-4 mr-2 mt-1.5 dark:rounded-2xl dark:bg-twitch"
        />
      </p>
      <p
        v-show="root"
        @click="del(props.threadId, props.postId)"
        class="font-bold hover:cursor-pointer"
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
        v-if="props.opcountposts"
        @click="openThread(theme, board)"
        class="hover:text-twitch cursor-pointer"
      >
        posts: {{ opcountposts }}
      </p>
    </div>

    <div class="gap-2 flex flex-col sm:flex-row">
      <div v-show="props.url"  class="gap-2 mt-2">
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

    <!-- Tooltip for displaying post preview -->
    <div
      v-if="hoverPost"
      :style="{ top: tooltipPosition.top + 'px', left: tooltipPosition.left + 'px' }"
      class="fixed bg-black dark:bg-twitch text-white p-2 rounded-2xl shadow-lg max-w-md"
    >
      <!-- Upper part of the tooltip with post info -->
      <div class="flex flex-wrap gap-1">
        <p>{{ hoverPost.theme }}</p>
        <p>{{ hoverPost.name }}</p>
        <p>{{ hoverPost.passcode }}</p>
        <p>{{ hoverPost.time }}</p>
        <p>{{ hoverPost.data }}</p>
        <p>#{{ hoverPost.postId.slice(12, 20) }}</p>
      </div>

      <!-- Post content text -->
      <div>
        <p class="pl-4 pt-2 pb-2">{{ hoverPost.text }}</p>
      </div>
    </div>
  </div>
</template>