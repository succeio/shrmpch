<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue';

const isVisible = ref(false);
const isScrollDownVisible = ref(false);

const handleScroll = () => {
  isVisible.value = window.scrollY > 400; // 
  isScrollDownVisible.value = window.scrollY < document.body.scrollHeight - window.innerHeight - 400; // Показывать кнопку "Вниз", если не внизу страницы
};

const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: 'smooth' });
};

const scrollToBottom = () => {
  window.scrollTo({ top: document.body.scrollHeight, behavior: 'smooth' });
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
});

onBeforeUnmount(() => {
  window.removeEventListener('scroll', handleScroll);
});
</script>

<template>
  <div>
    <!-- Ваш контент здесь -->

    <button
      v-if="isVisible"
      @click="scrollToTop"
      class="fixed top-6 right-6 dark:bg-twitch text-white rounded-full p-0 z-50 opacity-35 hover:opacity-55"
    >
        <img
          src="../assets/up.svg"
          alt="Icon"
          class="h-8 w-8 dark:rounded-full dark:bg-twitch"
        />
    </button>

    <button
      v-if="isScrollDownVisible"
      @click="scrollToBottom"
      class="fixed bottom-6 right-6 dark:bg-twitch text-white rounded-full p-0 z-50 opacity-35 hover:opacity-55"
    >
        <img
          src="../assets/down.svg"
          alt="Icon"
          class="h-8 w-8 dark:rounded-full dark:bg-twitch"
        />

    </button>
  </div>
</template>

