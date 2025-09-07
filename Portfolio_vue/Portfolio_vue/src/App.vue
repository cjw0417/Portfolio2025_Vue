<template>
  <nav class="gnb">
    <div class="gnb-title">Portfolio</div>
    <ul>
      <li v-for="item in menu" :key="item.index">
        <button @click="goToSlide(item.index)">{{ item.name }}</button>
      </li>
    </ul>
  </nav>
  <swiper
    :modules="[Mousewheel, Keyboard]"
    direction="vertical"
    :slides-per-view="1"
    :mousewheel="{ forceToAxis: true }"
    :keyboard="{ enabled: true }"
    :speed="1200"
    class="portfolio-swiper"
    @swiper="onSwiper"
  >
    <swiper-slide><Intro /></swiper-slide>
    <swiper-slide><About /></swiper-slide>
    <swiper-slide><Projects /></swiper-slide>
    <swiper-slide><Contact /></swiper-slide>
  </swiper>
</template>

<script setup>
import { ref } from 'vue'
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Mousewheel, Keyboard } from 'swiper/modules'
import 'swiper/css'
import 'swiper/css/mousewheel'
import 'swiper/css/keyboard'

import Intro from './components/Intro.vue'
import About from './components/About.vue'
import Projects from './components/Projects.vue'
import Contact from './components/Contact.vue'

const swiperInstance = ref(null)
const menu = [
  { name: 'Intro', index: 0 },
  { name: 'About', index: 1 },
  { name: 'Projects', index: 2 },
  { name: 'Contact', index: 3 },
]

const onSwiper = (swiper) => {
  swiperInstance.value = swiper
}

const goToSlide = (idx) => {
  if (swiperInstance.value && typeof swiperInstance.value.slideTo === 'function') {
    swiperInstance.value.slideTo(idx)
  }
}
</script>

<style></style>
