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
  <div class="flex justify-center w-full p-4">
    <button @click="switchTheme"
            class="cursor-pointer font-bold dark:text-white"
    >
      {{light ? 'Dark' : 'Light'}} Theme
    </button>
  </div>
</template>
