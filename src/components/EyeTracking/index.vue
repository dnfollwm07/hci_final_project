<!-- src/components/EyeTracking.vue -->
<template>
  <div id="tracking-area">
    <p>注视点 X: {{ gazeX }}</p>
    <p>注视点 Y: {{ gazeY }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, onUnmounted, ref } from 'vue'

export default defineComponent({
  name: 'EyeTracking',
  setup() {
    const gazeX = ref<number | null>(null)
    const gazeY = ref<number | null>(null)

    let interval: number | null = null

    onMounted(() => {
      if (typeof window.webgazer !== 'undefined') {
        window.webgazer
          .setRegression('ridge')
          .setTracker('clmtrackr')
          .showVideoPreview(true)
          .showPredictionPoints(true)
          .showFaceOverlay(true)
          .begin()

        interval = window.setInterval(async () => {
          const prediction = await window.webgazer.getCurrentPrediction()
          if (prediction) {
            gazeX.value = prediction.x
            gazeY.value = prediction.y
          }
        }, 100)
      }
    })

    onUnmounted(() => {
      if (interval) {
        clearInterval(interval)
        interval = null
      }
      if (typeof window.webgazer !== 'undefined') {
        window.webgazer.end()
      }
    })

    return {
      gazeX,
      gazeY
    }
  }
})
</script>

<style scoped>
#tracking-area {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
