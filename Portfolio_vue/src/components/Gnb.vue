<template>
  <nav class="gnb">
    <div class="gnb_title">Portfolio</div>
    <button class="gnb_hamburger" @click="open_menu = true" v-if="is_mobile">
      <span></span><span></span><span></span>
    </button>
    <ul v-else>
      <li v-for="item in menu" :key="item.text">
        <button @click="move_to(item.target)">{{ item.text }}</button>
      </li>
    </ul>
    <!-- 오버레이 메뉴 -->
    <div class="gnb_overlay" v-if="open_menu">
      <button class="gnb_close" @click="open_menu = false">×</button>
      <ul>
        <li v-for="item in menu" :key="item.text">
          <button
            @click="
              move_to(item.target);
              open_menu = false
            "
          >
            {{ item.text }}
          </button>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const menu = [
  { text: 'Intro', target: 'intro' },
  { text: 'About', target: 'about' },
  { text: 'Projects', target: 'projects' },
  { text: 'Contact', target: 'contact' },
]
const open_menu = ref(false)
const is_mobile = ref(window.innerWidth <= 1024)

const handle_resize = () => {
  is_mobile.value = window.innerWidth <= 1024
  if (!is_mobile.value) open_menu.value = false
}
onMounted(() => window.addEventListener('resize', handle_resize))
onUnmounted(() => window.removeEventListener('resize', handle_resize))

function move_to(target) {
  const el = document.querySelector(`.${target}`)
  if (el) el.scrollIntoView({ behavior: 'smooth' })
}
</script>

<style scoped>
/* 햄버거 버튼 */
.gnb_hamburger {
  display: none;
  flex-direction: column;
  justify-content: center;
  width: 38px;
  height: 38px;
  background: none;
  border: none;
  cursor: pointer;
  z-index: 1201;
  margin-left: auto;
}
.gnb_hamburger span {
  display: block;
  height: 4px;
  width: 28px;
  background: #17494d;
  margin: 4px 0;
  border-radius: 2px;
  transition: 0.3s;
}

/* 오버레이 메뉴 */
.gnb_overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  z-index: 1200;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  animation: fadeIn 0.3s;
}
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
.gnb_overlay ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.gnb_overlay li {
  margin: 24px 0;
}
.gnb_overlay button {
  font-size: 2rem;
  background: none;
  border: none;
  color: #fff;
  font-weight: bold;
  cursor: pointer;
  transition: color 0.2s;
}
.gnb_overlay button:hover {
  color: #90e7f3;
}
.gnb_close {
  position: absolute;
  top: 32px;
  right: 32px;
  font-size: 3rem;
  background: none;
  border: none;
  color: #fff;
  cursor: pointer;
}

/* 1024px 이하에서 햄버거 노출, 메뉴 숨김 */
@media (max-width: 1024px) {
  .gnb_hamburger {
    display: flex;
  }
}
</style>
