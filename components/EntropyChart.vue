<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue'

const props = withDefaults(defineProps<{
  width?: number
  height?: number
}>(), { width: 600, height: 280 })

// Entropy data: simulates normal → attack → recovery
const rawData = [
  3.8, 3.9, 3.7, 3.85, 3.6, 3.75, 3.8, 3.7, 3.5, 3.6,
  3.4, 3.2, 2.9, 2.6, 2.3, 1.8, 1.4, 1.1, 0.9, 0.7,
  0.8, 1.0, 1.3, 1.6, 2.0, 2.4, 2.8, 3.1, 3.4, 3.6
]

const THRESHOLD = 2.5
const maxEntropy = 4.2
const paddingLeft = 55
const paddingRight = 20
const paddingTop = 25
const paddingBottom = 40

const chartW = computed(() => props.width - paddingLeft - paddingRight)
const chartH = computed(() => props.height - paddingTop - paddingBottom)

function scaleX(i: number): number {
  return paddingLeft + (i / (rawData.length - 1)) * chartW.value
}
function scaleY(v: number): number {
  return paddingTop + (1 - v / maxEntropy) * chartH.value
}

// Main line path
const linePath = computed(() => {
  return rawData.map((v, i) => `${i === 0 ? 'M' : 'L'}${scaleX(i).toFixed(1)},${scaleY(v).toFixed(1)}`).join(' ')
})

// Danger zone polygon (area below threshold)
const dangerArea = computed(() => {
  const thresholdY = scaleY(THRESHOLD)
  const bottom = scaleY(0)
  // Find segments below threshold
  const points: string[] = []
  let inDanger = false
  for (let i = 0; i < rawData.length; i++) {
    if (rawData[i] <= THRESHOLD) {
      if (!inDanger) {
        // Interpolate entry point
        if (i > 0 && rawData[i - 1] > THRESHOLD) {
          const ratio = (THRESHOLD - rawData[i - 1]) / (rawData[i] - rawData[i - 1])
          const entryX = scaleX(i - 1 + ratio)
          points.push(`M${entryX.toFixed(1)},${thresholdY.toFixed(1)}`)
        } else {
          points.push(`M${scaleX(i).toFixed(1)},${thresholdY.toFixed(1)}`)
        }
        inDanger = true
      }
      points.push(`L${scaleX(i).toFixed(1)},${scaleY(rawData[i]).toFixed(1)}`)
    } else if (inDanger) {
      // Interpolate exit point
      const ratio = (THRESHOLD - rawData[i - 1]) / (rawData[i] - rawData[i - 1])
      const exitX = scaleX(i - 1 + ratio)
      points.push(`L${exitX.toFixed(1)},${thresholdY.toFixed(1)}`)
      points.push('Z')
      inDanger = false
    }
  }
  if (inDanger) {
    points.push(`L${scaleX(rawData.length - 1).toFixed(1)},${thresholdY.toFixed(1)}`)
    points.push('Z')
  }
  return points.join(' ')
})

// Animation: reveal line
const pathLength = ref(2000)
const dashOffset = ref(2000)
let animFrame: number

onMounted(() => {
  const start = performance.now()
  const duration = 2000
  function animate(now: number) {
    const elapsed = now - start
    const progress = Math.min(elapsed / duration, 1)
    const eased = 1 - Math.pow(1 - progress, 3)
    dashOffset.value = pathLength.value * (1 - eased)
    if (progress < 1) animFrame = requestAnimationFrame(animate)
  }
  animFrame = requestAnimationFrame(animate)
})

onUnmounted(() => { cancelAnimationFrame(animFrame) })

// Y-axis ticks
const yTicks = [0, 1, 2, 3, 4]
</script>

<template>
  <svg :viewBox="`0 0 ${width} ${height}`" class="entropy-chart" xmlns="http://www.w3.org/2000/svg">
    <defs>
      <linearGradient id="lineGrad" x1="0" y1="0" x2="1" y2="0">
        <stop offset="0%" stop-color="#28A745" />
        <stop offset="35%" stop-color="#28A745" />
        <stop offset="45%" stop-color="#DC3545" />
        <stop offset="70%" stop-color="#DC3545" />
        <stop offset="85%" stop-color="#28A745" />
        <stop offset="100%" stop-color="#28A745" />
      </linearGradient>
    </defs>

    <!-- Grid lines -->
    <line v-for="t in yTicks" :key="'g'+t"
          :x1="paddingLeft" :y1="scaleY(t)" :x2="width - paddingRight" :y2="scaleY(t)"
          stroke="#E2E8F0" stroke-width="0.7" />

    <!-- Y-axis labels -->
    <text v-for="t in yTicks" :key="'yl'+t"
          :x="paddingLeft - 10" :y="scaleY(t) + 4"
          text-anchor="end" font-size="11" fill="#6C7A89" font-family="Inter,sans-serif">
      {{ t.toFixed(1) }}
    </text>

    <!-- Y-axis title -->
    <text :x="14" :y="height / 2" text-anchor="middle" font-size="11" fill="#1B3A5C"
          font-family="Inter,sans-serif" font-weight="600"
          :transform="`rotate(-90, 14, ${height / 2})`">
      Entropy (H)
    </text>

    <!-- X-axis label -->
    <text :x="width / 2" :y="height - 5" text-anchor="middle" font-size="11" fill="#6C7A89" font-family="Inter,sans-serif">
      Time (sample index)
    </text>

    <!-- Danger fill area -->
    <path :d="dangerArea" fill="#DC3545" fill-opacity="0.12" />

    <!-- Threshold line -->
    <line :x1="paddingLeft" :y1="scaleY(THRESHOLD)" :x2="width - paddingRight" :y2="scaleY(THRESHOLD)"
          stroke="#DC3545" stroke-width="1.5" stroke-dasharray="8,4" />
    <text :x="width - paddingRight + 4" :y="scaleY(THRESHOLD) + 4"
          font-size="10" fill="#DC3545" font-family="Inter,sans-serif" font-weight="600">
      θ = {{ THRESHOLD }}
    </text>

    <!-- Main entropy line -->
    <path :d="linePath" fill="none" stroke="url(#lineGrad)" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"
          :stroke-dasharray="pathLength" :stroke-dashoffset="dashOffset" />

    <!-- Data points -->
    <circle v-for="(v, i) in rawData" :key="'dot'+i"
            :cx="scaleX(i)" :cy="scaleY(v)" r="3"
            :fill="v <= THRESHOLD ? '#DC3545' : '#28A745'"
            :opacity="dashOffset < pathLength * (1 - i / rawData.length) ? 1 : 0"
            stroke="#fff" stroke-width="1.5" />

    <!-- Labels -->
    <rect :x="scaleX(10) - 35" :y="scaleY(3.4) - 18" width="70" height="20" rx="4" fill="#28A745" fill-opacity="0.1"/>
    <text :x="scaleX(10)" :y="scaleY(3.4) - 5" text-anchor="middle" font-size="9" fill="#28A745" font-weight="600" font-family="Inter,sans-serif">
      ✅ Normal
    </text>

    <rect :x="scaleX(18) - 35" :y="scaleY(0.5) - 18" width="70" height="20" rx="4" fill="#DC3545" fill-opacity="0.1"/>
    <text :x="scaleX(18)" :y="scaleY(0.5) - 5" text-anchor="middle" font-size="9" fill="#DC3545" font-weight="600" font-family="Inter,sans-serif">
      🚨 Attack
    </text>
  </svg>
</template>

<style scoped>
.entropy-chart { width: 100%; max-width: 620px; height: auto; display: block; margin: 0 auto; }
</style>
