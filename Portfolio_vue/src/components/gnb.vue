<template>
  <nav class="gnb">
    <div class="gnb_inner">
      <div class="gnb_title">Menu</div>
      <button class="gnb_hamburger" @click="toggleMenu" aria-label="메뉴 열기" v-show="isMobile">
        <span></span>
        <span></span>
        <span></span>
      </button>
      <transition name="gnb_overlay_fade">
        <div v-if="menuOpen && isMobile" class="gnb_overlay">
          <button class="gnb_close" @click="toggleMenu" aria-label="메뉴 닫기">&times;</button>
          <ul>
            <li v-for="item in menu" :key="item.index">
              <button @click="handleMenuClick(item.index)">
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

onMounted(() => {
  window.addEventListener('resize', handleResize)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize)
})
</script>

<style scoped>
.gnb {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background: rgba(255, 255, 255, 0.9);
  z-index: 1000;
}
.gnb_inner {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 4rem;
  position: relative;
}
.gnb_title {
  font-weight: bold;
  font-size: 1.3rem;
  letter-spacing: 0.1em;
  flex-shrink: 0;
}
.gnb_menu_pc {
  display: flex;
  gap: 2rem;
  padding: 0;
  margin: 0;
  list-style: none;
}
.gnb button {
  background: none;
  border: none;
  font-size: 1.1rem;
  cursor: pointer;
}

/* 햄버거 버튼 */
.gnb_hamburger {
  display: none;
  flex-direction: column;
  justify-content: center;
  width: 32px;
  height: 32px;
  background: none;
  border: none;
  cursor: pointer;
  gap: 5px;
  margin-left: 1rem;
}
.gnb_hamburger span {
  display: block;
  height: 3px;
  width: 100%;
  background: #222;
  border-radius: 2px;
  transition: 0.3s;
}

/* 오버레이 메뉴 */
.gnb_overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(255,255,255,0.97);
  z-index: 2000;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.gnb_overlay ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 2.5rem;
}
.gnb_overlay button {
  font-size: 2rem;
  font-weight: 600;
  background: none;
  border: none;
  cursor: pointer;
  color: #222;
  transition: color 0.2s;
}
.gnb_overlay button:hover {
  color: #0078ff;
}

/* 닫기 버튼 */
.gnb_close {
  position: absolute;
  top: 2rem;
  right: 2rem;
  font-size: 3rem;
  background: none;
  border: none;
  color: #222;
  cursor: pointer;
  z-index: 2100;
  line-height: 1;
  padding: 0;
  transition: color 0.2s;
}
.gnb_close:hover {
  color: #0078ff;
}

/* 트랜지션 */
.gnb_overlay_fade-enter-active,
.gnb_overlay_fade-leave-active {
  transition: opacity 0.25s;
}
.gnb_overlay_fade-enter-from,
.gnb_overlay_fade-leave-to {
  opacity: 0;
}

/* 모바일 스타일 */
@media (max-width: 1000px) {
  .gnb_inner {
    padding: 0.5rem 1rem;
  }
  .gnb_hamburger {
    display: flex;
  }
  .gnb_menu_pc {
    display: none;
  }
}
</style>