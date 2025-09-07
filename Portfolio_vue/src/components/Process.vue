<template>
  <section class="process">
    <h2>My Process</h2>
    <ul>
      <li>1. 요구사항 분석</li>
      <li>2. UI/UX 설계</li>
      <li>3. 퍼블리싱 & Vue.js 개발</li>
      <li>4. 테스트 & 유지보수</li>
    </ul>
  </section>
</template>

<style scoped>
.process {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: #ddd;
}
.process ul {
  list-style: none;
  padding: 0;
}
.process li {
  margin: 10px 0;
}
</style>
