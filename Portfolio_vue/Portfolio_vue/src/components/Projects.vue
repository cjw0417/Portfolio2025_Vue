// ...existing code...
<template>
  <section class="projects">
    <div class="project_tip">Slide / Scroll "Cards" to see more projects</div>
    <div class="project_stack" ref="stackRef">
      <div
        v-for="(project, idx) in projects"
        :key="idx"
        class="project_card"
        :class="{ active: idx === current, stacked: idx < current }"
        :style="{ zIndex: idx + 1 }"
        @click="handleCardClick(idx)"
        style="cursor: pointer"
      >
        <!-- NH Pay 프로젝트일 때만 목업 슬라이드 -->
        <template v-if="project.title === 'NH Pay 다국어 프로젝트'">
          <div class="nhpay-mockup-slider">
            <img
              :src="nhpayMockups[nhpayCurrent]"
              alt="NH Pay 목업"
              class="project_img nhpay_img"
            />
          </div>
        </template>
        <!-- 그 외 프로젝트는 기존 이미지 -->
        <img
          v-else-if="project.img"
          :src="project.img"
          :alt="project.title + ' 이미지'"
          class="project_img"
        />
        <h3 v-html="project.title"></h3>
        <h4 v-html="project.subtit"></h4>
      </div>
    </div>
    <div class="project_desc_area">
      <p class="project_desc">{{ projects[current].desc }}</p>
      <p class="project_desc2">{{ projects[current].desc2 }}</p>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

const projects = [
  {
    title: '신한카드 운영',
    subtit: '2022.04 ~ 2024.12',
    desc: '신한카드 웹 유지보수 운영 및 신규 서비스 퍼블리싱',
    desc2: '#HTML · #CSS · #JavaScript · #jQuery · #Figma · #Swiper',
    img: '/shinhancard.png',
    url: 'https://www.shinhancard.com',
  },
  {
    title: '카드신청 비대면 신분증 인증',
    subtit: '2024.05 ~ 2024.11',
    desc: '비대면 신분증 인증 시스템 UI/UX 퍼블리싱',
    desc2: '#HTML · #CSS · #JavaScript · #jQuery · #Figma',
    img: '/OCR.png',
    url: '/OCR/main.html',
  },
  {
    title: '사내 페이지 리뉴얼',
    subtit: '2025.12 ~ 2025.03',
    desc: '홈페이지 리뉴얼 퍼블리싱',
    desc2: '#HTML · #CSS · #JavaScript · #jQuery · #Figma',
    img: '/m4a.png',
    url: 'https://m4a.co.kr',
  },
  {
    title: 'NH Pay 다국어 프로젝트',
    subtit: '2025.03 ~ 현재',
    desc: 'NH Pay 글로벌 서비스 다국어 퍼블리싱',
    desc2: '#다국어, #Vue.js, #SCSS',

    img: null,
    url: null,
  },
]

const nhpayMockups = [
  '/NHPay_mockup1.png',
  '/NHPay_mockup2.png',
  '/NHPay_mockup3.png',
  '/NHPay_mockup4.png',
  '/NHPay_mockup5.png',
]
const nhpayCurrent = ref(0)
let nhpayTimer = null

const current = ref(0)
const stackRef = ref(null)
let wheelLock = false

const next = () => {
  if (current.value < projects.length - 1) current.value++
}
const prev = () => {
  if (current.value > 0) current.value--
}

const handleCardClick = (idx) => {
  const url = projects[idx].url
  if (url) {
    if (url === '/OCR/main.html') {
      // 모바일 사이즈 새 창(360x740)
      window.open(url, '_blank', 'width=380,height=760')
    } else {
      window.open(url, '_blank')
    }
  }
}

// NH Pay 목업 자동 슬라이드
function startNhpaySlider() {
  clearInterval(nhpayTimer)
  nhpayCurrent.value = 0
  nhpayTimer = setInterval(() => {
    nhpayCurrent.value = (nhpayCurrent.value + 1) % nhpayMockups.length
    console.log('현재 목업 인덱스:', nhpayCurrent.value)
  }, 1500)
}

function stopNhpaySlider() {
  clearInterval(nhpayTimer)
}

watch(current, (val) => {
  // NH Pay 카드일 때만 슬라이드 시작
  if (projects[val].title === '농협 NH Pay 다국어') {
    startNhpaySlider()
  } else {
    stopNhpaySlider()
  }
})

onMounted(() => {
  const el = stackRef.value
  let startX = 0
  if (!el) return
  el.addEventListener(
    'wheel',
    (e) => {
      e.preventDefault()
      e.stopPropagation()
      if (wheelLock) return
      wheelLock = true
      if (e.deltaY > 0) next()
      else prev()
      setTimeout(() => {
        wheelLock = false
      }, 500)
    },
    { passive: false },
  )
  el.addEventListener('touchstart', (e) => {
    startX = e.touches[0].clientX
  })
  el.addEventListener('touchend', (e) => {
    const endX = e.changedTouches[0].clientX
    if (startX - endX > 30) next()
    else if (endX - startX > 30) prev()
  })
  // 최초 진입 시 NH Pay면 슬라이드 시작
  if (projects[current.value].title === '농협 NH Pay 다국어') {
    startNhpaySlider()
  }
})
</script>

<style scoped></style>
