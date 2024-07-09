<template>
  <div class="not-found">
    <h1>404</h1>
    <p>Oops! The page you are looking for does not exist.</p>
    <router-link to="/">Go back to Home</router-link>
    <div class="animation">
      <img src="/images/icon-404.svg" alt="Sad Icon" />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const imageUrl = ref(require("/images/icon-404.svg"));
</script>

