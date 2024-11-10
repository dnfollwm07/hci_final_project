<!-- src/App.vue 或 src/views/Home.vue -->
<template>
  <div id="app">
    <h1>眼动追踪演示</h1>
    <EyeTracking /> <!-- 使用 EyeTracking 组件 -->
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import EyeTracking from '../../../components/EyeTracking/index.vue'

export default defineComponent({
  name: 'App',
  components: {
    EyeTracking
  }
})
</script>
