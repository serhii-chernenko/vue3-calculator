<script setup>
import ThemeSwitcher from './ThemeSwitcher.vue';
import Button from './Button.vue';

const reset = () => {
  console.log('reset');
}
const divide = () => {
  console.log('divide');
}
</script>
<template>
  <ThemeSwitcher/>
  <div class="flex justify-center items-center min-h-screen">
    <div class="mt-4 p-4 rounded-md border border-gray-600 bg-gray-300 drop-shadow dark:bg-gray-600 dark:border-gray-300">
      <div class="flex items-center justify-end min-h-[56px] p-4 rounded-md bg-orange-400 shadow-inner font-bold">0</div>
      <div class="grid grid-cols-4 gap-1 mt-4 justify-between">
        <Button @click="reset">c</Button>
        <div></div>
        <div></div>
        <Button @click="divide">÷</Button>
        <Button>7</Button>
        <Button>8</Button>
        <Button>9</Button>
        <Button>×</Button>
        <Button>4</Button>
        <Button>5</Button>
        <Button>6</Button>
        <Button>–</Button>
        <Button>1</Button>
        <Button>2</Button>
        <Button>3</Button>
        <Button>+</Button>
        <Button>0</Button>
        <div></div>
        <Button>.</Button>
        <Button>=</Button>
      </div>
    </div>
  </div>
</template>
