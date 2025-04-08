<!-- components/header/Sections.vue -->
<template>
    <div class="sections">
      <div v-for="item in categories" :key="item.semanticId">
        <button class="sectionsBtn" type="button" @click="$emit('change-layout', item.semanticId)">
          {{ item.name }}
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      categories: {
        type: Array,
        required: true
      }
    }
  }
  </script>
  
  <style lang="scss">
  .sections {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 1000px;
  }
  
  .sections > div:not(:last-child) {
    position: relative;
    margin-right: 20px;
  }
  
  .sections > div:not(:last-child)::after {
    content: '';
    position: absolute;
    top: 0;
    right: -100px;
    width: 2px;
    height: 100%;
    background-color: #000000;
  }
  
  .sectionsBtn {
    font-size: $headerFontSize;
    transition: transform 0.3s ease;
  }
  
  .sectionsBtn:hover {
    transform: scale(1.2);
  }
  </style>