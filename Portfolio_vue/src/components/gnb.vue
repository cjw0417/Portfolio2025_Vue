<template>
  <nav class="gnb">
    <div class="gnb_inner">
      <div class="gnb_title" @click="goToMain" style="cursor: pointer">Menu</div>
      <button class="gnb_hamburger" @click="toggleMenu" aria-label="메뉴 열기" v-show="isMobile">
        <span></span>
        <span></span>
        <span></span>
      </button>
      <transition name="gnb_overlay_slide">
        <div v-if="menuOpen && isMobile" class="gnb_overlay">
          <button class="gnb_close" @click="toggleMenu" aria-label="메뉴 닫기">&times;</button>
          <ul>
            <li v-for="item in menu" :key="item.index">
              <button @click="handleMenuClick(item.index)">
                <span v-if="item.name === 'Intro'">🏠</span>
                <span v-else-if="item.name === 'About'">👤</span>
                <span v-else-if="item.name === 'Projects'">💼</span>
                <span v-else-if="item.name === 'Contact'">✉️</span>
                {{ item.name }}
              </button>
            </li>
          </ul>
        </div>
      </transition>
      <ul v-if="!isMobile" class="gnb_menu_pc">
        <li v-for="item in menu" :key="item.index">
          <button @click="handleMenuClick(item.index)">
            {{ item.name }}
          </button>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script setup>
import { defineProps, ref, onMounted, onBeforeUnmount } from 'vue'
const props = defineProps({
  goToSlide: Function,
})

const menu = [
  { name: 'Intro', index: 0 },
  { name: 'About', index: 1 },
  { name: 'Projects', index: 2 },
  { name: 'Contact', index: 3 },
]

const isMobile = ref(window.innerWidth <= 1000)
const menuOpen = ref(false)

const handleResize = () => {
  isMobile.value = window.innerWidth <= 1000
  if (!isMobile.value) menuOpen.value = false
}

const toggleMenu = () => {
  menuOpen.value = !menuOpen.value
}

const handleMenuClick = (idx) => {
  props.goToSlide(idx)
  if (isMobile.value) menuOpen.value = false
}

// gnb_title 클릭 시 메인(인트로)로 이동
const goToMain = () => {
  props.goToSlide(0)
  if (isMobile.value) menuOpen.value = false
}

onMounted(() => {
  window.addEventListener('resize', handleResize)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize)
})
</script>
