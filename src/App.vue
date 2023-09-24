<script setup>
import { reactive, ref, onMounted, onBeforeUnmount } from "vue";
import ThemeSwitcher from "./ThemeSwitcher.vue";

const ACTIONS = {
  reset: "reset",
  result: "result",
};
const total = ref("");
const buttons = reactive([
  { title: "c", action: ACTIONS.reset, class: "col-span-3" },
  { title: "÷", action: "/" },
  { title: "7", action: "7" },
  { title: "8", action: "8" },
  { title: "9", action: "9" },
  { title: "×", action: "*" },
  { title: "4", action: "4" },
  { title: "5", action: "5" },
  { title: "6", action: "6" },
  { title: "-", action: "-" },
  { title: "1", action: "1" },
  { title: "2", action: "2" },
  { title: "3", action: "3" },
  { title: "+", action: "+" },
  { title: "0", action: "0", class: "col-span-2" },
  { title: ".", action: "." },
  { title: "=", action: ACTIONS.result },
]);

const copyToClipboard = () => {
  navigator.clipboard.writeText(total.value);
};

const handleClick = (value) => {
  switch (value) {
    case ACTIONS.reset:
      total.value = "";
      break;
    case ACTIONS.result:
      // If the last character is an operator, remove it
      if (total.value.slice(-1).match(/[-+*/]/)) {
        total.value = total.value.slice(0, -1);
      }

      total.value = eval(total.value);
      copyToClipboard();
      break;
    default:
      if (!total.value) {
        if (value === "0") {
          // Prevents adding zeros on the beginning
          total.value = "0.";
          break;
        } else if (value.match(/[-+*/]/)) {
          // Prevents adding operators on the beginning
          total.value = "";
        }
      } else if (
        total.value &&
        value.match(/[-+*/]/) &&
        total.value.slice(-1).match(/[-+*/]/)
      ) {
        // Prevents adding multiple operators

        if (value !== total.value.slice(-1)) {
          // If the last character is an operator, replace the previous one
          total.value = total.value.slice(0, -1) + value;
        }

        break;
      }

      total.value += value;
      break;
  }
};

const handlePress = (event) => {
  const keyValue = String.fromCharCode(event.keyCode);

  switch (event.keyCode) {
    case 13:
      handleClick(ACTIONS.result);
      break;
    case 8:
      handleClick(ACTIONS.reset);
      break;
    default:
      const button = Object.values(buttons).find(
        (button) => button.title.toLowerCase() === keyValue.toLowerCase(),
      );

      if (button) {
        handleClick(button.action);
      }

      break;
  }
};

onMounted(() => {
  window.addEventListener("keydown", handlePress);
});

onBeforeUnmount(() => {
  window.removeEventListener("keydown", handlePress);
});
</script>
<template>
  <div class="flex flex-col justify-center items-center min-h-screen">
    <ThemeSwitcher />
    <div class="flex flex-grow justify-center items-center">
      <div
        class="p-4 rounded-md border border-gray-600 bg-gray-300 drop-shadow dark:bg-gray-600 dark:border-gray-300"
      >
        <div
          @click="copyToClipboard"
          class="flex items-center justify-end max-w-[204px] min-h-[56px] p-4 rounded-md bg-orange-400 shadow-inner font-bold whitespace-nowrap overflow-hidden"
        >
          {{ total }}
        </div>
        <div class="grid grid-cols-4 gap-1 mt-4 justify-between">
          <button
            v-for="button in buttons"
            :key="button.action"
            :class="button.class"
            @click="handleClick(button.action)"
            class="flex items-center justify-center w-12 h-12 m-0 rounded-md bg-gray-200 drop-shadow dark:bg-gray-800 font-bold dark:text-white focus:shadow-inner"
          >
            {{ button.title }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
