<template>
  <swiper
    direction="vertical"
    :slides-per-view="1"
    :mousewheel="true"
    :keyboard="true"
    class="portfolio-swiper"
  >
    <swiper-slide><Intro /></swiper-slide>
    <swiper-slide><About /></swiper-slide>
    <swiper-slide><Projects /></swiper-slide>
    <swiper-slide><Process /></swiper-slide>
    <swiper-slide><Contact /></swiper-slide>
  </swiper>
</template>

<script setup>
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css'
import 'swiper/css/mousewheel'
import 'swiper/css/keyboard'

import Intro from './components/Intro.vue'
import About from './components/About.vue'
import Projects from './components/Projects.vue'
import Process from './components/Process.vue'
import Contact from './components/Contact.vue'
</script>

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
</style>
