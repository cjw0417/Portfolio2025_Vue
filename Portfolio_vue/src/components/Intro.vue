<template>
  <section class="intro">
    <h1 class="intro_tit" :class="{ 'animate-down': show }">PORTFOLIO</h1>
    <p class="intro_txt" :class="{ 'animate-up': show }">
      안녕하세요.<br />
      실무에 강한 퍼블리셔 <span class="highlight">조재우</span>입니다.
    </p>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
const show = ref(false)

onMounted(() => {
  setTimeout(() => {
    show.value = true
  }, 800)
})
</script>
