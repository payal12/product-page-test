<template>
  <button :class="[buttonClass]" @click="handleClick" :disabled="disabled">
    <slot></slot>
  </button>
</template>
  
<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  buttonClass: {
    type: String,
    default: "",
  },
});

const emit = defineEmits(["click"]);

const handleClick = (event) => {
  emit("click", event);
};
</script>