<script setup>
import { ref, inject, watch, onMounted, onUnmounted} from 'vue'
import { useRoute, useRouter } from 'vue-router';
import { database, storage } from '../firebase'
import { ref as dbRef, push, set, get, update, remove } from 'firebase/database'
import { ref as storageRef, uploadBytes, getDownloadURL } from 'firebase/storage'
import ErrorNotification from './ErrorNotification.vue';

const imagePreview = ref(null);
const mimeType = ref(null); // Сохраняем MIME-тип
const fileInput = ref(null);
const maxFileSize = 4 * 1024 * 1024; // Максимальный размер файла: 4MB
const allowedFormats = ['image/svg+xml', 'image/jpeg', 'image/png', 'image/gif', 'image/webp', 'video/mp4', 'video/webm'];

const handlePaste = async (event) => {
  const items = event.clipboardData.items;
  for (let item of items) {
    const file = item.getAsFile();
    if (file) {
      // Проверяем формат и размер файла
      const error = validateFile(file);
      if (error) {
        alert(error); // Выводим ошибку
      } else {
        await uploadFile(file);
      }
    }
  }
};

const handleFileUpload = async (event) => {
  const file = event.target.files[0];
  if (file) {
    // Проверяем формат и размер файла
    const error = validateFile(file);
    if (error) {
      //alert(error); // Выводим ошибку
    } else {
      await uploadFile(file);
    }
  }
};

const openFileDialog = () => {
  fileInput.value.click();
};

// Функция для проверки формата и размера файла
const validateFile = (file) => {
  if (!allowedFormats.includes(file.type)) {
    errorMessage.value = 'Неподдерживаемый формат файла. Допустимы только изображения и видео (jpeg, png, gif, webp, mp4, webm, svg).';
    errorTrigger.value++; // Обновляем триггер
    return
  }
  if (file.size > maxFileSize) {
    errorMessage.value = `Размер файла превышает 4MB. Текущий размер: ${(file.size / 1024 / 1024).toFixed(2)}MB.`;
    errorTrigger.value++; // Обновляем триггер
    return 
  }
  return null; // Если ошибок нет
};

const uploadFile = async (file) => {
  const uniqueFileName = `${Date.now()}-${file.name}`;
  const storageReference = storageRef(storage, `uploads/${route.params.board}/${route.params.thread}/${uniqueFileName}`);

  // Загружаем файл в Firebase Storage
  await uploadBytes(storageReference, file);

  // Получаем прямую ссылку на загруженный файл
  const downloadURL = await getDownloadURL(storageReference);
  imagePreview.value = downloadURL; // Устанавливаем URL для предпросмотра
  mimeType.value = file.type; // Сохраняем MIME-тип
  postUrl.value = downloadURL;
};

onMounted(() => {
  window.addEventListener('paste', handlePaste);
});

onUnmounted(() => {
  window.removeEventListener('paste', handlePaste);
});

const errorMessage = ref('');
const errorTrigger = ref(0); 

const route = useRoute();
const router = useRouter();


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

const isSubmitting = ref(false);

const submitPost = async () => {
  if (isSubmitting.value) return;
  
  isSubmitting.value = true;

  try {
    // Ожидаем завершения отправки поста
    await sendPost();
  } catch (error) {
    console.error("Ошибка при отправке поста:", error);
  } finally {
    // Разблокируем кнопку в любом случае (успех или ошибка)
    isSubmitting.value = false;
  }
}

const sendPost = async () => {
  try {

function _0x1b31(){const _0x42b4b6=['value','banned/','4423149FyCIxr','288588RAidVK','toLocaleString','/uIds','params','val','uId\x20','10OUaUET','4Tjqwjy','uId','board','45846xsxfMC','798005dUwXWA','15277812Qcixbr','https://api.ipify.org?format=json','/uIds/','189cEalbP','json','3yykhyy','exp','3787415HqSRpk','exists','557832WNUXgC'];_0x1b31=function(){return _0x42b4b6;};return _0x1b31();}const _0x3154a6=_0x522d;(function(_0x153f0d,_0x1e27bf){const _0x3eac01=_0x522d,_0x1b9081=_0x153f0d();while(!![]){try{const _0x51d89f=-parseInt(_0x3eac01(0x139))/0x1+parseInt(_0x3eac01(0x12e))/0x2*(-parseInt(_0x3eac01(0x126))/0x3)+-parseInt(_0x3eac01(0x135))/0x4*(parseInt(_0x3eac01(0x128))/0x5)+parseInt(_0x3eac01(0x138))/0x6*(parseInt(_0x3eac01(0x124))/0x7)+parseInt(_0x3eac01(0x12a))/0x8+parseInt(_0x3eac01(0x12d))/0x9+parseInt(_0x3eac01(0x134))/0xa*(parseInt(_0x3eac01(0x121))/0xb);if(_0x51d89f===_0x1e27bf)break;else _0x1b9081['push'](_0x1b9081['shift']());}catch(_0x302fb8){_0x1b9081['push'](_0x1b9081['shift']());}}}(_0x1b31,0x6f79f));const response=await fetch(_0x3154a6(0x122)),data=await response[_0x3154a6(0x125)](),ipAddress=data['ip'];function _0x522d(_0x140f8f,_0x1b3414){const _0x1b3137=_0x1b31();return _0x522d=function(_0x522dfd,_0x40e23b){_0x522dfd=_0x522dfd-0x121;let _0x563468=_0x1b3137[_0x522dfd];return _0x563468;},_0x522d(_0x140f8f,_0x1b3414);}uId[_0x3154a6(0x12b)]=await hashString(ipAddress);const bannedRef=dbRef(database,_0x3154a6(0x12c)+route[_0x3154a6(0x131)][_0x3154a6(0x137)]+_0x3154a6(0x130)),bannedSnapshot=await get(bannedRef),ropopom=ref(![]),banExpiration=ref(null);if(bannedSnapshot[_0x3154a6(0x129)]()){const bannedData=bannedSnapshot[_0x3154a6(0x132)](),banKey=ref(null);for(const key in bannedData){if(bannedData[key][_0x3154a6(0x136)]===uId[_0x3154a6(0x12b)]){ropopom[_0x3154a6(0x12b)]=!![],banExpiration[_0x3154a6(0x12b)]=bannedData[key][_0x3154a6(0x127)],banKey[_0x3154a6(0x12b)]=key;break;}}if(ropopom['value']){const currentTime=Date['now']();if(currentTime<banExpiration['value'])console['log'](_0x3154a6(0x133)+uId[_0x3154a6(0x12b)]+'\x20забанен\x20до\x20'+new Date(banExpiration['value'])[_0x3154a6(0x12f)]());else{const banKeyRef=dbRef(database,_0x3154a6(0x12c)+route['params'][_0x3154a6(0x137)]+_0x3154a6(0x123)+banKey[_0x3154a6(0x12b)]);await remove(banKeyRef),ropopom[_0x3154a6(0x12b)]=![];}}}    

//-------      

    if (ropopom.value === false) {
      if (
        route.params.thread &&
        route.params.board &&
        boards.some((board) => route.params.board.includes(board))
      ) {
        const postId = push(dbRef(database, `${route.params.board}/${route.params.thread}`)).key // Генерация уникального ID

        replies.value = postText.value.match(/#([A-Za-z0-9_-]+)/g)

        loadImg()

        hashedString.value = await hashString(postPassword.value)
        const newPost = {
          name: postName.value ? (postName.value.length > 25 ? postName.value.slice(0, 25) : postName.value) : 'Аноним',
          password: postPassword.value ? hashedString.value : '',
          theme: postTheme.value.length < 45 ? postTheme.value : postTheme.value.slice(0, 25),
          text: (/\s{4,}/.test(postText.value) ? postText.value.replace(/\s{4,}/g, ' ') : postText.value).replace(/[^A-Za-zА-Яа-я0-9\s\w\s.ё—`,:;!?'"<>\\//{[]}$#(@%^&*_+=)-]/g, ''),
          url: postUrl.value,
          time: new Date().toLocaleTimeString('ru-RU', {
            timeZone: 'Europe/Moscow',
            hour: '2-digit',
            minute: '2-digit',
            second: '2-digit'
          }),
          data: new Date().toLocaleDateString('ru-RU', { day: '2-digit', month: '2-digit', year: 'numeric' }).replace(/\//g, '.'),
          day: ['Вс', 'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб'][new Date().getDay()],
          postId: postId,
          threadId: route.params.thread,
          uId: uId.value,
          mimeType: typeof mimeType.value === 'string' ? mimeType.value : ''
        }

        const savedTime = localStorage.getItem('tmlg')
          ? localStorage.getItem('tmlg')
          : localStorage.setItem('tmlg', Date.now() + 6)
        if (savedTime) {
          const currentTimeElapsed = Date.now() - savedTime
          if (currentTimeElapsed >= 5000) {
            if (postText.value.length < 450 && selectedEmoji.value === generatedEmoji.value) {
              // Сохраняем новый пост
              await set(dbRef(database, `${route.params.board}/${route.params.thread}/posts/${postId}`), newPost)
              localStorage.setItem('tmlg', Date.now())
              imagePreview.value = ""

              try {

                const snapshot = await get(dbRef(database, `${route.params.board}/${route.params.thread}`))
                if (snapshot.exists()) {
                  const data = snapshot.val()
                  if (data.lastPostTimestamp != 9999999999999) {
                    // ----------- Обновление lastPostTimestamp в треде ----------- 
                    await update(dbRef(database, `${route.params.board}/${route.params.thread}`), {
                      lastPostTimestamp: Date.now() // Обновляем только метку времени последнего поста
                    })
                  }
                }
              } catch (err) {
                console.error(`Ошибка при обновлении метки последнего поста в треде: `, err)
              }

              // ----------- код обновления reply ----------- 
              if (replies.value && replies.value.length) {
                for (const id of replies.value) {
                  const sId = id.replace('#', '')
                  const postRef = dbRef(database, `${route.params.board}/${route.params.thread}/posts/${sId}`)

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
      } else if (route.params.board && boards.includes(route.params.board)) {
        const threadId = push(dbRef(database, 'threads')).key // Генерация уникального ID для нового треда
        const postId = push(dbRef(database, `${route.params.board}/${threadId}`)).key // Генерация уникального ID для поста

        hashedString.value = await hashString(postPassword.value)
        const newPost = {
          name: postName.value ? (postName.value.length > 25 ? postName.value.slice(0, 25) : postName.value) : 'Аноним',
          password: postPassword.value ? hashedString.value : '',
          theme: postTheme.value.length < 45 ? postTheme.value : postTheme.value.slice(0, 25),
          text: (/\s{4,}/.test(postText.value) ? postText.value.replace(/\s{4,}/g, ' ') : postText.value).replace(/[^A-Za-zА-Яа-я0-9\s\w\sё—`.,:;!?'"<>\\//{[]}$#(@%^&*_+=)-]/g, ''),
          url: postUrl.value,
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
          uId: uId.value,
          mimeType: typeof mimeType.value === 'string' ? mimeType.value : ''
        }

        const savedTime = localStorage.getItem('tmlg')
          ? localStorage.getItem('tmlg')
          : localStorage.setItem('tmlg', Date.now() + 6)
        if (savedTime) {
          const currentTimeElapsed = Date.now() - savedTime
          if (currentTimeElapsed >= 25000) {
            if (postText.value.length < 450 && (/\.(jpeg|jpg|gif|png|mp4|webm|ogg)$/i.test(postUrl.value) || postUrl.value.includes('fire') ) && selectedEmoji.value === generatedEmoji.value) {
              await set(dbRef(database, `${route.params.board}/${threadId}/posts/${postId}`), newPost)
              imagePreview.value = ""

              // ----------- Установка lastPostTimestamp и 0p для нового треда ----------- 
              await update(dbRef(database, `${route.params.board}/${threadId}`), {
                lastPostTimestamp: Date.now(), // Обновляем только метку времени последнего поста
                op: newPost
              })

              generateEmojis()
              postText.value = ''
              postUrl.value = ''
              postTheme.value = ''

              router.push({ path: `/${route.params.board}/${threadId}` })
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
      } else {
        errorMessage.value = `Раздел не существует.`
        errorTrigger.value++; // Обновляем триггер        
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

(function(_0x1a7223,_0x3eaf98){const _0x35134d=_0x389d,_0x32053d=_0x1a7223();while(!![]){try{const _0x3c28b=parseInt(_0x35134d(0x147))/0x1+parseInt(_0x35134d(0x145))/0x2+parseInt(_0x35134d(0x151))/0x3*(-parseInt(_0x35134d(0x144))/0x4)+parseInt(_0x35134d(0x14e))/0x5*(parseInt(_0x35134d(0x150))/0x6)+-parseInt(_0x35134d(0x149))/0x7+parseInt(_0x35134d(0x148))/0x8*(parseInt(_0x35134d(0x14a))/0x9)+-parseInt(_0x35134d(0x153))/0xa*(parseInt(_0x35134d(0x154))/0xb);if(_0x3c28b===_0x3eaf98)break;else _0x32053d['push'](_0x32053d['shift']());}catch(_0x473164){_0x32053d['push'](_0x32053d['shift']());}}}(_0x2e32,0x6c435));function _0x389d(_0x2aa0f4,_0x23aaed){const _0x2e327d=_0x2e32();return _0x389d=function(_0x389df6,_0x4a38){_0x389df6=_0x389df6-0x140;let _0x1ad402=_0x2e327d[_0x389df6];return _0x1ad402;},_0x389d(_0x2aa0f4,_0x23aaed);}function _0x2e32(){const _0x2ea504=['18WQyocy','toString','subtle','padStart','18040Fvzvoe','substring','942lCaKJK','29658Zkkoxg','digest','290bkTipj','151327xVYWQO','encode','join','map','SHA-256','56omwRAX','1727424kmpzow','from','53467ZkqkrQ','454176TXBkXU','4314639gSqFDZ'];_0x2e32=function(){return _0x2ea504;};return _0x2e32();}const hashString=async _0x50f653=>{const _0xda1a15=_0x389d,_0x18bc43=new TextEncoder(),_0x4835f0=_0x18bc43[_0xda1a15(0x140)](_0x50f653),_0x4078dc=await crypto[_0xda1a15(0x14c)][_0xda1a15(0x152)](_0xda1a15(0x143),_0x4835f0),_0x133e76=Array[_0xda1a15(0x146)](new Uint8Array(_0x4078dc)),_0x19f4fe=_0x133e76[_0xda1a15(0x142)](_0x2cf427=>_0x2cf427[_0xda1a15(0x14b)](0x10)[_0xda1a15(0x14d)](0x2,'0'))[_0xda1a15(0x141)]('');return _0x19f4fe[_0xda1a15(0x14f)](0x8,0x10);};


watch(
  () => props.replyId,
  (newReplyId) => {
    if (newReplyId) { // проверка на null
      postText.value = `${postText.value} #${String(newReplyId)}`;
    }
  }
);


(function(_0x21a4c7,_0x59564d){const _0x3d37ac=_0x78ee,_0x2dc8fe=_0x21a4c7();while(!![]){try{const _0x13da8c=-parseInt(_0x3d37ac(0x1c7))/0x1*(parseInt(_0x3d37ac(0x1c3))/0x2)+parseInt(_0x3d37ac(0x1c4))/0x3+-parseInt(_0x3d37ac(0x1ca))/0x4*(-parseInt(_0x3d37ac(0x1bf))/0x5)+-parseInt(_0x3d37ac(0x1c9))/0x6+parseInt(_0x3d37ac(0x1cf))/0x7*(parseInt(_0x3d37ac(0x1cb))/0x8)+parseInt(_0x3d37ac(0x1c5))/0x9+-parseInt(_0x3d37ac(0x1c0))/0xa*(parseInt(_0x3d37ac(0x1cd))/0xb);if(_0x13da8c===_0x59564d)break;else _0x2dc8fe['push'](_0x2dc8fe['shift']());}catch(_0x406af8){_0x2dc8fe['push'](_0x2dc8fe['shift']());}}}(_0x46a5,0x9e4b6));const emojisPool=ref(['😀','😂','😍','😎','😢','🥳','🤔','😱','😴']),generatedEmoji=ref(null),selectedEmoji=ref(null),resultMessage=ref(''),emojis=ref([]),generateEmojis=()=>{const _0x3dbbf4=_0x78ee,_0x17e99c=[...emojisPool[_0x3dbbf4(0x1c1)]];generatedEmoji[_0x3dbbf4(0x1c1)]=_0x17e99c[Math[_0x3dbbf4(0x1bd)](Math['random']()*_0x17e99c[_0x3dbbf4(0x1c2)])];const _0x587c3a=new Set();_0x587c3a['add'](generatedEmoji[_0x3dbbf4(0x1c1)]);while(_0x587c3a[_0x3dbbf4(0x1c6)]<0x3){_0x587c3a[_0x3dbbf4(0x1ce)](_0x17e99c[Math[_0x3dbbf4(0x1bd)](Math[_0x3dbbf4(0x1c8)]()*_0x17e99c['length'])]);}emojis['value']=Array[_0x3dbbf4(0x1bb)](_0x587c3a)[_0x3dbbf4(0x1bc)](()=>Math[_0x3dbbf4(0x1c8)]()-0.5),selectedEmoji[_0x3dbbf4(0x1c1)]=null,resultMessage['value']='';},selectEmoji=_0x23c540=>{const _0x416ac9=_0x78ee;selectedEmoji[_0x416ac9(0x1c1)]=_0x23c540,localStorage['setItem'](_0x416ac9(0x1cc),_0x23c540),checkSelection();},checkSelection=()=>{const _0x2ed27b=_0x78ee;selectedEmoji[_0x2ed27b(0x1c1)]===generatedEmoji[_0x2ed27b(0x1c1)]?resultMessage[_0x2ed27b(0x1c1)]=_0x2ed27b(0x1be):resultMessage[_0x2ed27b(0x1c1)]='Попробуйте\x20снова!';};function _0x78ee(_0x38c4bf,_0x3815cc){const _0x46a504=_0x46a5();return _0x78ee=function(_0x78ee28,_0x27ed9c){_0x78ee28=_0x78ee28-0x1bb;let _0x56cf94=_0x46a504[_0x78ee28];return _0x56cf94;},_0x78ee(_0x38c4bf,_0x3815cc);}function _0x46a5(){const _0x4885b5=['floor','Капча\x20решена!','6348455gFwLTi','340bphtav','value','length','28300AUqABj','1827165vZngZl','2585520hKgAoj','size','23mkeJhX','random','5003334LeAaJX','4dlbjwn','8YyuSmk','selectedEmoji','350438CYEkdo','add','5074013iBcPTL','from','sort'];_0x46a5=function(){return _0x4885b5;};return _0x46a5();}

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
      <div class=" flex flex-col mt-2">
        <div class="relative">
<textarea
          @keyup.shift.enter="submitPost"
          v-model="postText"
          placeholder="post"
          class="min-h-24 w-full text-sm p-2 ring-1 ring-slate-900/10 shadow-sm rounded-md focus:outline-none focus:ring-2 focus:ring-pink-500 caret-pink-500 dark:bg-zinc-800 dark:ring-0 dark:highlight-white/5 dark:focus:ring-2 dark:focus:ring-pink-500 dark:focus:bg-zinc-900 dark:text-white"
          rows="4"
        ></textarea>

        <div
          :class="{
            'text-red-500': postText.length > 450,
            'text-zinc-300': postText.length <= 450,
            'select-none': true
          }"
          class="absolute bottom-2 right-2 text-xs"
        >
          {{ postText.length }}/450
        </div>          
      </div>
              
  <div 
    class="w-full p-4 border-dashed border border-gray-300 rounded-md  hover:cursor-pointer text-zinc-500 text-sm"
    @click="openFileDialog"
  >
    <input type="file" ref="fileInput" @change="handleFileUpload" style="display: none;" />
    <p v-show="!imagePreview">Нажмите или вставьте изображение из буфера</p>
    <img v-if="imagePreview && /svg|jpeg|jpg|gif|png|JPG|JPEG|GIF|PNG|SVG/.test(postUrl)" :src="imagePreview" alt="Preview" class=" w-1/3 h-auto rounded-2xl" />
    <video v-if="imagePreview && /mp4|webm|ogg/.test(postUrl)" :src="imagePreview"  class=" w-1/3 h-auto rounded-2xl"></video>
   
  </div>

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

           <button v-if="imagePreview"
            @click="imagePreview = '', postUrl = ''"
            class="dark:bg-twitch bg-black text-white rounded-full p-1 w-full lg:w-8"
          >
            X
          </button>          


          <button
             :disabled="isSubmitting"
            @click="submitPost"
            class="dark:bg-twitch bg-black dark:disabled:bg-zinc-900 disabled:bg-gray-300 text-white rounded-2xl p-1 w-full lg:min-w-32 lg:w-auto"
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
