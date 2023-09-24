<script setup>
import { ref, onMounted } from 'vue';

const light = ref(null);

onMounted(() => {
  if (localStorage.theme === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
    document.documentElement.classList.add('dark');
    localStorage.setItem('theme', 'dark');
    light.value = false;
  } else {
    document.documentElement.classList.remove('dark');
    localStorage.removeItem('theme');
    light.value = true;
  }
});

const switchTheme = () => {
  if (light.value) {
    document.documentElement.classList.add('dark');
    localStorage.setItem('theme', 'dark');
    light.value = false;

    return false;
  }

  document.documentElement.classList.remove('dark');
  localStorage.removeItem('theme');
  light.value = true;
}
</script>
<template>
  <button @click="switchTheme"
          class="absolute top-4 w-full cursor-pointer font-bold dark:text-white"
  >
    {{light ? 'dark' : 'light'}} theme
  </button>
</template>
