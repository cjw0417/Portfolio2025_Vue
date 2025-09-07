// ...existing code...
<template>
  <section class="projects">
    <div class="project_stack" ref="stackRef">
      <div
        v-for="(project, idx) in projects"
        :key="project.title"
        class="project_card"
        :class="{ active: idx === current, stacked: idx < current }"
        :style="{ zIndex: idx + 1 }"
      >
        <!-- NH Pay 프로젝트일 때만 목업 슬라이드 -->
        <template v-if="project.title === '농협 NH Pay 다국어'">
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
        <h3>{{ project.title }}</h3>
      </div>
    </div>
    <div class="project_desc_area">
      <p class="project_desc">{{ projects[current].desc }}</p>
      <div class="project_tip">wheel / swipe to show more</div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

const projects = [
  {
    title: '신한카드 운영',
    desc: '신한카드 웹사이트 유지보수 및 신규 서비스 퍼블리싱',
    img: '/shinhancard.png',
  },
  {
    title: '비대면 신분증 인증',
    desc: '비대면 신분증 인증 시스템 UI/UX 퍼블리싱',
    img: '/OCR.png',
  },
  { title: '페이지 리뉴얼', desc: '기업 홈페이지 리뉴얼 퍼블리싱', img: '/m4a.png' },
  {
    title: '농협 NH Pay 다국어',
    desc: 'NH Pay 글로벌 서비스 다국어 퍼블리싱',
    img: null, // 이미지 직접 사용하지 않음
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
