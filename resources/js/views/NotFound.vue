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

<style scoped>
.not-found {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  text-align: center;
  background: linear-gradient(135deg, #f06, #ffba00);
  color: white;
  font-family: "Arial", sans-serif;
}

.not-found h1 {
  font-size: 8rem;
  margin: 0;
  color: #fff;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4);
}

.not-found p {
  font-size: 1.5rem;
  margin: 20px 0;
}

.not-found a {
  font-size: 1.2rem;
  color: #ffba00;
  text-decoration: none;
  border: 2px solid #ffba00;
  padding: 10px 20px;
  border-radius: 5px;
  transition: background-color 0.3s, color 0.3s;
}

.not-found a:hover {
  background-color: #ffba00;
  color: #f06;
}

.animation {
  margin-top: 30px;
}

.animation img {
  width: 200px;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-20px);
  }
}
</style>
