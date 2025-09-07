<template>
  <section class="projects">
    <h2>Projects</h2>
    <swiper :slides-per-view="1" :loop="true" :autoplay="{ delay: 2500 }" class="inner-swiper">
      <swiper-slide class="project">신한카드 운영</swiper-slide>
      <swiper-slide class="project">비대면 신분증 인증</swiper-slide>
      <swiper-slide class="project">페이지 리뉴얼</swiper-slide>
      <swiper-slide class="project">농협 NH Pay 다국어</swiper-slide>
    </swiper>
  </section>
</template>

<script setup>
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css'
import 'swiper/css/autoplay'
</script>

<style scoped>
.projects {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: #ddd;
}
.inner-swiper {
  width: 300px;
  height: 400px;
  margin-top: 20px;
}
.project {
  display: flex;
  justify-content: center;
  align-items: center;
  background: white;
  font-size: 1.2rem;
  border-radius: 10px;
}
</style>
