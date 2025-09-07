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
import Process from './components/Process.vue'
import Contact from './components/Contact.vue'

const swiperRef = ref(null)
const menu = [
  { name: 'Intro', index: 0 },
  { name: 'About', index: 1 },
  { name: 'Projects', index: 2 },
  { name: 'Process', index: 3 },
  { name: 'Contact', index: 4 },
]

const goToSlide = (idx) => {
  if (swiperRef.value) {
    swiperRef.value.swiper.slideTo(idx)
  }
}
</script>

<template>
  <nav class="gnb">
    <ul>
      <li v-for="item in menu" :key="item.index">
        <button @click="goToSlide(item.index)">{{ item.name }}</button>
      </li>
    </ul>
  </nav>
  <swiper
    ref="swiperRef"
    direction="vertical"
    :slides-per-view="1"
    :modules="[Mousewheel, Keyboard]"
    :mousewheel="{ forceToAxis: true }"
    :keyboard="{ enabled: true }"
    :speed="1200"
    class="portfolio-swiper"
  >
    <swiper-slide><Intro /></swiper-slide>
    <swiper-slide><About /></swiper-slide>
    <swiper-slide><Projects /></swiper-slide>
    <swiper-slide><Process /></swiper-slide>
    <swiper-slide><Contact /></swiper-slide>
  </swiper>
</template>

<style>
html,
body,
#app {
  margin: 0;
  height: 100%;
}
.portfolio-swiper {
  width: 100%;
  height: 100vh;
}
.gnb {
  position: fixed;
  width: 50%;
  top: 0;
  right: 0;
  background: transparent;
  z-index: 1000;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
}
.gnb ul {
  display: flex;
  justify-content: center;
  gap: 32px;
  margin: 0;
  padding: 16px 0;
  list-style: none;
}
.gnb button {
  background: none;
  border: none;
  font-size: 1.3rem;
  cursor: pointer;
  padding: 8px 16px;
  transition: color 0.2s;
}
.gnb button:hover {
  color: #007bff;
}
</style>
