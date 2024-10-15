<script setup>
import { ref, inject, watch, onMounted } from 'vue'
import { database } from '../firebase'
import { ref as dbRef, push, set, get, update, remove } from 'firebase/database'
import ErrorNotification from './ErrorNotification.vue';

const errorMessage = ref('');
const errorTrigger = ref(0); // Триггер для обновления компонента ошибки


;(function () {
  const devtools = { orientation: false }
  window.addEventListener('resize', function () {
    if (
      window.outerWidth - window.innerWidth > 100 ||
      window.outerHeight - window.innerHeight > 100
    ) {
      devtools.orientation = true
      generatedEmoji.value = ' '
    }
  })
})()

const props = defineProps({
  replyId: String
})

const fetchPosts = inject('fetchPosts')

const postName = ref('')
const postTheme = ref('')
const postText = ref('')
const postUrl = ref('')
const postPassword = ref('')
const hashedString = ref('')
const threadState = ref('')
const boardState = ref('')
const replies = ref([])
const uId = ref('')

const imgSize = ref(0)

const boards = [
  'Anime',
  'Asylum',
  'b',
  'dev',
  'vg',
  'vn',
  'gd',
  'Art',
  'cg',
  'Manga',
  'md',
  'bg',
  'Paranormal',
  'pr',
  'tech'
]

const sendPost = async () => {
  try {
    threadState.value = localStorage.getItem('threadState')
      ? localStorage.getItem('threadState')
      : localStorage.setItem('threadState', '')
    boardState.value = localStorage.getItem('boardState')
      ? localStorage.getItem('boardState')
      : localStorage.setItem('boardState', '')

//--------

    const response = await fetch('https://api.ipify.org?format=json')
    const data = await response.json()
    const data0 = data.ip; 
    uId.value = await hashString(data0); 

    const bannedRef = dbRef(database, `banned/${boardState.value}/uIds`);
    const bannedSnapshot = await get(bannedRef);
    const isBanned = ref(false)
    const banExpiration = ref(null)

    if (bannedSnapshot.exists()) {
      const bannedData = bannedSnapshot.val();
      const banKey = ref(null)

      for (const key in bannedData) {
        if (bannedData[key].uId === uId.value) {
          isBanned.value = true;
          banExpiration.value = bannedData[key].exp;
          banKey.value = key; 
          break;
        }
      }

      if (isBanned.value) {
        const currentTime = Date.now();
        if (currentTime < banExpiration.value) {
          console.log(`Забанен до ${new Date(banExpiration.value).toLocaleString()}`);
        } else {
          const banKeyRef = dbRef(database, `banned/${boardState.value}/uIds/${banKey.value}`);
          await remove(banKeyRef); 
          isBanned.value = false
        }
      }
    }

//-------      

    if (isBanned.value === false) {
      if (
        threadState.value &&
        boardState.value &&
        boards.some((board) => boardState.value.includes(board))
      ) {
        const postId = push(dbRef(database, `${boardState.value}/${threadState.value}`)).key // Генерация уникального ID

        replies.value = postText.value.match(/#([A-Za-z0-9_-]+)/g)

        loadImg()

        hashedString.value = await hashString(postPassword.value)
        const newPost = {
          name: postName.value ? (postName.value.length > 25 ? postName.value.slice(0, 25) : postName.value) : 'Аноним',
          password: postPassword.value ? hashedString.value : '',
          theme: postTheme.value.length < 45 ? postTheme.value : postTheme.value.slice(0, 25),
          text: (/\s{4,}/.test(postText.value) ? postText.value.replace(/\s{4,}/g, ' ') : postText.value).replace(/[^A-Za-zА-Яа-я0-9\s\w\s.,:;!?'"<>\\//{}$#(@%^&*_+=)-]/g, ''),
          url: postUrl.value.length < 100 ? (imgSize.value !== 0 ? (imgSize.value < 4000000 ? postUrl.value : '') : postUrl.value) : '',
          time: new Date().toLocaleTimeString('ru-RU', {
            timeZone: 'Europe/Moscow',
            hour: '2-digit',
            minute: '2-digit',
            second: '2-digit'
          }),
          data: new Date().toLocaleDateString('ru-RU', { day: '2-digit', month: '2-digit', year: 'numeric' }).replace(/\//g, '.'),
          day: ['Вс', 'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб'][new Date().getDay()],
          postId: postId,
          threadId: threadState.value,
          uId: uId.value
        }

        const savedTime = localStorage.getItem('tmlg')
          ? localStorage.getItem('tmlg')
          : localStorage.setItem('tmlg', Date.now() + 6)
        if (savedTime) {
          const currentTimeElapsed = Date.now() - savedTime
          if (currentTimeElapsed >= 5000) {
            if (postText.value.length < 450 && selectedEmoji.value === generatedEmoji.value) {
              // Сохраняем новый пост
              await set(dbRef(database, `${boardState.value}/${threadState.value}/posts/${postId}`), newPost)
              localStorage.setItem('tmlg', Date.now())

              try {
                // ----------- Обновление lastPostTimestamp в треде ----------- 
                await update(dbRef(database, `${boardState.value}/${threadState.value}`), {
                  lastPostTimestamp: Date.now() // Обновляем только метку времени последнего поста
                })
              } catch (err) {
                console.error(`Ошибка при обновлении метки последнего поста в треде: `, err)
              }

              // ----------- код обновления reply ----------- 
              if (replies.value && replies.value.length) {
                for (const id of replies.value) {
                  const sId = id.replace('#', '')
                  const postRef = dbRef(database, `${boardState.value}/${threadState.value}/posts/${sId}`)

                  try {
                    const snapshot = await get(postRef)
                    if (snapshot.exists()) {
                      const currentReplies = snapshot.val().replies || []
                      if (!currentReplies.includes(postId)) {
                        await update(postRef, {
                          replies: [...currentReplies, postId]
                        })
                        console.log(`Пост с id ${sId} успешно обновлен!`)
                      } else {
                        console.log(`Пост с id ${sId} уже содержит newPostId.`)
                      }
                    } else {
                      console.log(`Пост с id ${sId} не найден.`)
                    }
                  } catch (err) {
                    console.error(`Ошибка при обновлении документа с id ${sId}: `, err)
                  }
                }
              }
              // ----------- end -----------

              setTimeout(() => {
                window.scrollTo({
                  top: document.body.scrollHeight,
                  behavior: 'smooth'
                })
              }, 100)
              generateEmojis()

              postText.value = ''
              postUrl.value = ''
              postTheme.value = ''

            } else {
              errorMessage.value = 'Пост слишком длинный либо не решена капча! Максимальная длина 450 символов.'
              errorTrigger.value++; // Обновляем триггер
            }
          } else {
            errorMessage.value = 'Подождите 5 сек.'
            errorTrigger.value++; // Обновляем триггер          
          }
        }
      } else if (boardState.value && boards.some((board) => boardState.value.includes(board))) {
        const threadId = push(dbRef(database, 'threads')).key // Генерация уникального ID для нового треда
        const postId = push(dbRef(database, `${boardState.value}/${threadId}`)).key // Генерация уникального ID для поста

        hashedString.value = await hashString(postPassword.value)
        const newPost = {
          name: postName.value ? (postName.value.length > 25 ? postName.value.slice(0, 25) : postName.value) : 'Аноним',
          password: postPassword.value ? hashedString.value : '',
          theme: postTheme.value.length < 45 ? postTheme.value : postTheme.value.slice(0, 25),
          text: (/\s{4,}/.test(postText.value) ? postText.value.replace(/\s{4,}/g, ' ') : postText.value).replace(/[^A-Za-zА-Яа-я0-9\s\w\s.,:;!?'"<>\\//{}$#(@%^&*_+=)-]/g, ''),
          url: postUrl.value.length < 100 ? postUrl.value : '',
          time: new Date().toLocaleTimeString('ru-RU', {
            timeZone: 'Europe/Moscow',
            hour: '2-digit',
            minute: '2-digit',
            second: '2-digit'
          }),
          data: new Date().toLocaleDateString('ru-RU', { day: '2-digit', month: '2-digit', year: 'numeric' }).replace(/\//g, '.'),
          day: ['Вс', 'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб'][new Date().getDay()],
          postId: postId,
          threadId: threadId,
          uId: uId.value
        }

        const savedTime = localStorage.getItem('tmlg')
          ? localStorage.getItem('tmlg')
          : localStorage.setItem('tmlg', Date.now() + 6)
        if (savedTime) {
          const currentTimeElapsed = Date.now() - savedTime
          if (currentTimeElapsed >= 25000) {
            if (postText.value.length < 450 && /\.(jpeg|jpg|gif|png|mp4|webm|ogg)$/i.test(postUrl.value) && selectedEmoji.value === generatedEmoji.value) {
              await set(dbRef(database, `${boardState.value}/${threadId}/posts/${postId}`), newPost)

              // ----------- Установка lastPostTimestamp и 0p для нового треда ----------- 
              await update(dbRef(database, `${boardState.value}/${threadId}`), {
                lastPostTimestamp: Date.now(), // Обновляем только метку времени последнего поста
                op: newPost
              })


              localStorage.setItem('threadState', threadId)
              threadState.value = threadId

              generateEmojis()
              postText.value = ''
              postUrl.value = ''
              postTheme.value = ''

              fetchPosts()
            } else {
              errorMessage.value = 'Пост слишком длинный или не выбран файл! Максимальная длина 450 символов.';
              errorTrigger.value++; // Обновляем триггер
            }
          } else {
            errorMessage.value = 'Подождите 25 сек.';
            errorTrigger.value++; // Обновляем триггер
          }
        }
      }
    } else {
      errorMessage.value = `Забанен до ${new Date(banExpiration.value).toLocaleString()}`
      errorTrigger.value++; // Обновляем триггер
    }
  } catch (error) {
    console.error(error)
  }
}

const loadImg = () => {
    // Очищаем предыдущие значения
    imgSize.value = 0;

    // Получаем расширение файла
    const extension = postUrl.value.split('.').pop().toLowerCase();

    // Проверяем, является ли расширение допустимым
    if (['jpeg', 'jpg', 'gif', 'png'].includes(extension)) {
        const img = new Image();
        img.src = postUrl.value;

        // Обработчик события загрузки изображения
        img.onload = () => {
            imgSize.value = img.width * img.height; // Сохраняем размер изображения
            console.log(`Размер изображения: ${imgSize.value}`);
        };

        // Обработчик события ошибки загрузки изображения
        img.onerror = () => {
            console.error('Ошибка загрузки изображения по указанному URL');
            imgSize.value = 0; // Очищаем размер в случае ошибки
        };
    } else {
        //console.error('Недопустимый формат изображения');
        imgSize.value = 0; // Очищаем размер, если формат недопустимый
    }
} 

function _0x38d6(_0x410b09,_0x4f0b69){const _0x55bb3a=_0x55bb();return _0x38d6=function(_0x38d613,_0x2a15c9){_0x38d613=_0x38d613-0x18d;let _0x398822=_0x55bb3a[_0x38d613];return _0x398822;},_0x38d6(_0x410b09,_0x4f0b69);}(function(_0x1d9f3c,_0x3e5953){const _0xd21615=_0x38d6,_0x3f4258=_0x1d9f3c();while(!![]){try{const _0x34efff=-parseInt(_0xd21615(0x18f))/0x1+parseInt(_0xd21615(0x194))/0x2*(parseInt(_0xd21615(0x19d))/0x3)+-parseInt(_0xd21615(0x190))/0x4*(-parseInt(_0xd21615(0x197))/0x5)+-parseInt(_0xd21615(0x192))/0x6*(parseInt(_0xd21615(0x193))/0x7)+-parseInt(_0xd21615(0x18e))/0x8+-parseInt(_0xd21615(0x18d))/0x9+parseInt(_0xd21615(0x19b))/0xa;if(_0x34efff===_0x3e5953)break;else _0x3f4258['push'](_0x3f4258['shift']());}catch(_0x55c5d3){_0x3f4258['push'](_0x3f4258['shift']());}}}(_0x55bb,0x3dfd5));const hashString=async _0x2b2bb0=>{const _0x521242=_0x38d6,_0x51ec30=new TextEncoder(),_0x15ae5d=_0x51ec30[_0x521242(0x19c)](_0x2b2bb0),_0x4d1608=await crypto[_0x521242(0x199)]['digest']('SHA-256',_0x15ae5d),_0x20ee61=Array[_0x521242(0x196)](new Uint8Array(_0x4d1608)),_0x120f26=_0x20ee61[_0x521242(0x191)](_0x16d332=>_0x16d332[_0x521242(0x195)](0x10)[_0x521242(0x198)](0x2,'0'))[_0x521242(0x19a)]('');return _0x120f26['substring'](0x8,0x10);};function _0x55bb(){const _0x2fe74d=['padStart','subtle','join','11915860yLBget','encode','69WBIgoi','4494411DVAgWc','2929240WdcXFX','401101VTobtp','68wwLMni','map','1715556oZhXXx','7GbtEho','22574VnLyqc','toString','from','104495iwcRhe'];_0x55bb=function(){return _0x2fe74d;};return _0x55bb();}

watch(
  () => props.replyId,
  (newReplyId) => {
    if (newReplyId) { // проверка на null
      postText.value = `${postText.value} #${String(newReplyId)}`;
    }
  }
);

(function(_0xa99e1f,_0x1248b6){const _0x20fb25=_0x38a6,_0x1643be=_0xa99e1f();while(!![]){try{const _0x42fc7f=-parseInt(_0x20fb25(0xda))/0x1+parseInt(_0x20fb25(0xe7))/0x2*(-parseInt(_0x20fb25(0xe0))/0x3)+parseInt(_0x20fb25(0xe4))/0x4*(parseInt(_0x20fb25(0xd7))/0x5)+parseInt(_0x20fb25(0xe8))/0x6+-parseInt(_0x20fb25(0xd6))/0x7*(parseInt(_0x20fb25(0xe6))/0x8)+parseInt(_0x20fb25(0xdd))/0x9*(-parseInt(_0x20fb25(0xd4))/0xa)+parseInt(_0x20fb25(0xde))/0xb;if(_0x42fc7f===_0x1248b6)break;else _0x1643be['push'](_0x1643be['shift']());}catch(_0x139e0f){_0x1643be['push'](_0x1643be['shift']());}}}(_0x36e3,0xc0454));function _0x38a6(_0x4a4473,_0x4a1d4f){const _0x36e38f=_0x36e3();return _0x38a6=function(_0x38a67d,_0x14c51f){_0x38a67d=_0x38a67d-0xd3;let _0x47213b=_0x36e38f[_0x38a67d];return _0x47213b;},_0x38a6(_0x4a4473,_0x4a1d4f);}function _0x36e3(){const _0x3182d6=['size','1406134qdBhzs','floor','Капча\x20решена!','77175kNhjEr','4312308cwLalP','add','174OPlgkT','length','value','setItem','4859664WSGWuR','random','664XtyxDT','1768BJRqXv','6185742vVAIjc','sort','300FxBEHb','Попробуйте\x20снова!','11445eOvPzW','5qppMvc','from'];_0x36e3=function(){return _0x3182d6;};return _0x36e3();}const emojisPool=ref(['😀','😂','😍','😎','😢','🥳','🤔','😱','😴']),generatedEmoji=ref(null),selectedEmoji=ref(null),resultMessage=ref(''),emojis=ref([]),generateEmojis=()=>{const _0x202790=_0x38a6,_0x41e4ec=[...emojisPool[_0x202790(0xe2)]];generatedEmoji[_0x202790(0xe2)]=_0x41e4ec[Math[_0x202790(0xdb)](Math[_0x202790(0xe5)]()*_0x41e4ec[_0x202790(0xe1)])];const _0x4505fd=new Set();_0x4505fd[_0x202790(0xdf)](generatedEmoji[_0x202790(0xe2)]);while(_0x4505fd[_0x202790(0xd9)]<0x3){_0x4505fd['add'](_0x41e4ec[Math[_0x202790(0xdb)](Math[_0x202790(0xe5)]()*_0x41e4ec['length'])]);}emojis[_0x202790(0xe2)]=Array[_0x202790(0xd8)](_0x4505fd)[_0x202790(0xd3)](()=>Math['random']()-0.5),selectedEmoji['value']=null,resultMessage[_0x202790(0xe2)]='';},selectEmoji=_0x12178f=>{const _0x22b96f=_0x38a6;selectedEmoji[_0x22b96f(0xe2)]=_0x12178f,localStorage[_0x22b96f(0xe3)]('selectedEmoji',_0x12178f),checkSelection();},checkSelection=()=>{const _0x19c3a3=_0x38a6;selectedEmoji[_0x19c3a3(0xe2)]===generatedEmoji[_0x19c3a3(0xe2)]?resultMessage['value']=_0x19c3a3(0xdc):resultMessage[_0x19c3a3(0xe2)]=_0x19c3a3(0xd5);};

//function _0x1b08(_0x37a8db,_0x107182){const _0x5247fb=_0x5247();return _0x1b08=function(_0x1b08a7,_0x35ce14){_0x1b08a7=_0x1b08a7-0x83;let _0x53ee0e=_0x5247fb[_0x1b08a7];return _0x53ee0e;},_0x1b08(_0x37a8db,_0x107182);}(function(_0x18ae9e,_0xe2f842){const _0x37f534=_0x1b08,_0x44c8a8=_0x18ae9e();while(!![]){try{const _0x45d2dd=parseInt(_0x37f534(0x91))/0x1*(parseInt(_0x37f534(0x88))/0x2)+parseInt(_0x37f534(0x86))/0x3*(parseInt(_0x37f534(0x93))/0x4)+-parseInt(_0x37f534(0x95))/0x5*(parseInt(_0x37f534(0x87))/0x6)+parseInt(_0x37f534(0x8b))/0x7*(parseInt(_0x37f534(0x96))/0x8)+parseInt(_0x37f534(0x84))/0x9+-parseInt(_0x37f534(0x8e))/0xa*(-parseInt(_0x37f534(0x94))/0xb)+parseInt(_0x37f534(0x90))/0xc*(-parseInt(_0x37f534(0x8a))/0xd);if(_0x45d2dd===_0xe2f842)break;else _0x44c8a8['push'](_0x44c8a8['shift']());}catch(_0x14daf3){_0x44c8a8['push'](_0x44c8a8['shift']());}}}(_0x5247,0xba6b2));function _0x5247(){const _0x2b2358=['length','4216833DGNSiJ','value','3hAJtJE','82182nhKpZE','194078SjHQhk','random','6018883dBbjFZ','518QDpvQR','size','selectedEmoji','60gpixOR','add','12Kvspel','1JHacJH','floor','148872FYapqe','452837woTmDG','295kRHNFV','128096aXBqbJ','Попробуйте\x20снова!'];_0x5247=function(){return _0x2b2358;};return _0x5247();}const emojis=ref(['😀','😂','😍','😎','😢','🥳','🤔','😱','😴']),generatedEmoji=ref(null),selectedEmoji=ref(null),resultMessage=ref(''),generateEmojis=()=>{const _0x5cffc4=_0x1b08;generatedEmoji[_0x5cffc4(0x85)]=emojis[_0x5cffc4(0x85)][Math[_0x5cffc4(0x92)](Math[_0x5cffc4(0x89)]()*emojis[_0x5cffc4(0x85)][_0x5cffc4(0x83)])];const _0x3d0c81=new Set();_0x3d0c81[_0x5cffc4(0x8f)](generatedEmoji[_0x5cffc4(0x85)]);while(_0x3d0c81[_0x5cffc4(0x8c)]<0x3){_0x3d0c81[_0x5cffc4(0x8f)](emojis[_0x5cffc4(0x85)][Math[_0x5cffc4(0x92)](Math[_0x5cffc4(0x89)]()*emojis['value']['length'])]);}emojis[_0x5cffc4(0x85)]=Array['from'](_0x3d0c81),selectedEmoji[_0x5cffc4(0x85)]=null,resultMessage[_0x5cffc4(0x85)]='';},selectEmoji=_0x1f4ee5=>{const _0xb8ea93=_0x1b08;selectedEmoji[_0xb8ea93(0x85)]=_0x1f4ee5,localStorage['setItem'](_0xb8ea93(0x8d),_0x1f4ee5),checkSelection();},checkSelection=()=>{const _0x310bc1=_0x1b08;selectedEmoji['value']===generatedEmoji[_0x310bc1(0x85)]?resultMessage[_0x310bc1(0x85)]='Капча\x20решена!':resultMessage[_0x310bc1(0x85)]=_0x310bc1(0x97);};

onMounted(() => {
  localStorage.setItem('tmlg', Date.now() + 6)
}, generateEmojis())

const addBold = () => {
  const textarea = document.querySelector('textarea')
  const start = textarea.selectionStart
  const end = textarea.selectionEnd

  // Получаем выделенный текст
  const selectedText = postText.value.substring(start, end)

  if (selectedText) {
    // Обрамляем выделенный текст
    const wrappedText = `**${selectedText}**`

    // Обновляем текст с обрамленным выделением
    postText.value =
      postText.value.substring(0, start) + wrappedText + postText.value.substring(end)

    // Сбрасываем выделение
    textarea.focus()
    textarea.setSelectionRange(start, start + wrappedText.length)
  } else {
    // Если ничего не выбрано, добавляем ** **
    const wrappedText = '** **'
    postText.value =
      postText.value.substring(0, start) + wrappedText + postText.value.substring(end)

    // Устанавливаем курсор между звездочками
    textarea.focus()
    textarea.setSelectionRange(start + 3, start + 3) // Устанавливаем курсор между ** **
  }
}

const addCode = () => {
  const textarea = document.querySelector('textarea')
  const start = textarea.selectionStart
  const end = textarea.selectionEnd

  // Получаем выделенный текст
  const selectedText = postText.value.substring(start, end)

  if (selectedText) {
    // Обрамляем выделенный текст
    const wrappedText = `\`${selectedText}\``

    // Обновляем текст с обрамленным выделением
    postText.value =
      postText.value.substring(0, start) + wrappedText + postText.value.substring(end)

    // Сбрасываем выделение
    textarea.focus()
    textarea.setSelectionRange(start, start + wrappedText.length)
  } else {
    // Если ничего не выбрано, добавляем ` `
    const wrappedText = `\` \``
    postText.value =
      postText.value.substring(0, start) + wrappedText + postText.value.substring(end)

    // Устанавливаем курсор между звездочками
    textarea.focus()
    textarea.setSelectionRange(start + 3, start + 3) // Устанавливаем курсор между ` `
  }
}

const addItalic = () => {
  const textarea = document.querySelector('textarea')
  const start = textarea.selectionStart
  const end = textarea.selectionEnd

  // Получаем выделенный текст
  const selectedText = postText.value.substring(start, end)

  if (selectedText) {
    // Обрамляем выделенный текст
    const wrappedText = `*${selectedText}*`

    // Обновляем текст с обрамленным выделением
    postText.value =
      postText.value.substring(0, start) + wrappedText + postText.value.substring(end)

    // Сбрасываем выделение
    textarea.focus()
    textarea.setSelectionRange(start, start + wrappedText.length)
  } else {
    // Если ничего не выбрано, добавляем * *
    const wrappedText = `* *`
    postText.value =
      postText.value.substring(0, start) + wrappedText + postText.value.substring(end)

    // Устанавливаем курсор между звездочками
    textarea.focus()
    textarea.setSelectionRange(start + 3, start + 3) // Устанавливаем курсор между * *
  }
}

const addLink = () => {
  const textarea = document.querySelector('textarea')
  const start = textarea.selectionStart
  const end = textarea.selectionEnd

  // Получаем выделенный текст
  const selectedText = postText.value.substring(start, end)

  if (selectedText) {
    // Обрамляем выделенный текст
    const wrappedText = `[${selectedText}](${selectedText})`

    // Обновляем текст с обрамленным выделением
    postText.value =
      postText.value.substring(0, start) + wrappedText + postText.value.substring(end)

    // Сбрасываем выделение
    textarea.focus()
    textarea.setSelectionRange(start, start + wrappedText.length)
  } else {
    // Если ничего не выбрано, добавляем [ ]( )
    const wrappedText = `[ ]( )`
    postText.value =
      postText.value.substring(0, start) + wrappedText + postText.value.substring(end)

    // Устанавливаем курсор между звездочками
    textarea.focus()
    textarea.setSelectionRange(start + 3, start + 3) // Устанавливаем курсор между [ ]( )
  }
}

const addQuote = () => {
  const textarea = document.querySelector('textarea')
  const start = textarea.selectionStart
  const end = textarea.selectionEnd

  // Получаем выделенный текст
  const selectedText = postText.value.substring(start, end)

  if (selectedText) {
    // Обрамляем выделенный текст
    const wrappedText = `>${selectedText}`

    // Обновляем текст с обрамленным выделением
    postText.value =
      postText.value.substring(0, start) + wrappedText + postText.value.substring(end)

    // Сбрасываем выделение
    textarea.focus()
    textarea.setSelectionRange(start, start + wrappedText.length)
  } else {
    // Если ничего не выбрано, добавляем >
    const wrappedText = `>`
    postText.value =
      postText.value.substring(0, start) + wrappedText + postText.value.substring(end)

    // Устанавливаем курсор между звездочками
    textarea.focus()
    textarea.setSelectionRange(start + 3, start + 3) // Устанавливаем курсор между >
  }
}
</script>

<template>
  <div class="container p-4 lg:p-4 lg:w-full lg:mx-0 lg:pl-0">
    <div class="pl-4 form-group w-full lg:w-2/3">
      <!-- Поля ввода с адаптацией по разрешению -->
      <div class="flex flex-col lg:flex-row gap-2">
        <input
          v-model="postName"
          placeholder="name"
          class="w-full lg:flex-1 text-sm p-2 ring-1 ring-slate-900/10 shadow-sm rounded-md focus:outline-none focus:ring-2 focus:ring-pink-500 caret-pink-500 dark:bg-zinc-800 dark:ring-0 dark:highlight-white/5 dark:focus:ring-2 dark:focus:ring-pink-500 dark:focus:bg-zinc-900 dark:text-white"
          type="text"
        />
        <input
          v-model="postTheme"
          placeholder="theme"
          class="w-full lg:flex-1 text-sm p-2 ring-1 ring-slate-900/10 shadow-sm rounded-md focus:outline-none focus:ring-2 focus:ring-pink-500 caret-pink-500 dark:bg-zinc-800 dark:ring-0 dark:highlight-white/5 dark:focus:ring-2 dark:focus:ring-pink-500 dark:focus:bg-zinc-900 dark:text-white"
          type="text"
        />
        <input
          v-model="postPassword"
          placeholder="passcode"
          class="w-full lg:flex-1 text-sm p-2 ring-1 ring-slate-900/10 shadow-sm rounded-md focus:outline-none focus:ring-2 focus:ring-pink-500 caret-pink-500 dark:bg-zinc-800 dark:ring-0 dark:highlight-white/5 dark:focus:ring-2 dark:focus:ring-pink-500 dark:focus:bg-zinc-900 dark:text-white"
          type="text"
        />
      </div>

      <!-- Текстовое поле -->
      <div class="flex flex-col mt-2">
        <textarea
          @keyup.shift.enter="sendPost"
          v-model="postText"
          placeholder="post"
          class="w-full text-sm p-2 ring-1 ring-slate-900/10 shadow-sm rounded-md focus:outline-none focus:ring-2 focus:ring-pink-500 caret-pink-500 dark:bg-zinc-800 dark:ring-0 dark:highlight-white/5 dark:focus:ring-2 dark:focus:ring-pink-500 dark:focus:bg-zinc-900 dark:text-white"
          rows="4"
        ></textarea>
      </div>

      <!-- Кнопка и управление форматированием -->
      <div class="form-group mt-2">
        <div class="flex flex-col lg:flex-row items-start lg:items-center gap-2 mt-2">
          <input
            v-model="postUrl"
            placeholder="url"
            class="w-full lg:flex-1 text-sm p-2 ring-1 ring-slate-900/10 shadow-sm rounded-md focus:outline-none focus:ring-2 focus:ring-pink-500 caret-pink-500 dark:bg-zinc-800 dark:ring-0 dark:highlight-white/5 dark:focus:ring-2 dark:focus:ring-pink-500 dark:focus:bg-zinc-900 dark:text-white"
            type="text"
          />
          <button
            @click="sendPost"
            class="dark:bg-twitch bg-black text-white rounded-2xl p-1 w-full lg:min-w-32 lg:w-auto"
          >
            Post
          </button>

          <div class="flex gap-2 mt-2 lg:mt-0">
            <a href="https://imgur.com/upload" target="_blank" rel="noopener noreferrer">
              <img
                src="../assets/clip.svg"
                alt="Icon"
                class="p-1 w-8 dark:rounded-2xl dark:bg-twitch"
              />
            </a>

            <div class="flex gap-2">
              <button
                class="text-sm dark:bg-twitch bg-black text-white rounded-full p-1 w-8 h-8"
                @click="addBold"
              >
                B
              </button>
              <button
                class="text-sm dark:bg-twitch bg-black text-white rounded-full p-1 w-8 h-8"
                @click="addCode"
              >
                C
              </button>
              <button
                class="text-sm dark:bg-twitch bg-black text-white rounded-full p-1 w-8 h-8"
                @click="addItalic"
              >
                I
              </button>
              <button
                class="text-sm dark:bg-twitch bg-black text-white rounded-full p-1 w-8 h-8"
                @click="addLink"
              >
                L
              </button>
              <button
                class="dark:bg-twitch bg-black text-white rounded-2xl p-1 w-8 h-8"
                @click="addQuote"
              >
                Q
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Капча с эмодзи -->
      <div class="flex bg-black w-full lg:w-2/3 rounded-2xl text-white p-1 dark:bg-twitch mt-4">
        <p class="ml-2">Найдите {{ generatedEmoji }}:</p>
        <div class="flex gap-2 pl-4">
          <div
            v-for="(emoji, index) in emojis"
            :key="index"
            @click="selectEmoji(emoji)"
            class="cursor-pointer hover:scale-110 transition-transform"
          >
            {{ emoji }}
          </div>
        </div>
        <div class="pl-4" v-if="checkSelection">{{ resultMessage }}</div>
      </div>
    </div>

    <!-- Компонент для отображения ошибок -->
    <ErrorNotification :message="errorMessage" :trigger="errorTrigger" />
  </div>
</template>
