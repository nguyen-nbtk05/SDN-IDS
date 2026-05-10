<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'

const props = withDefaults(defineProps<{
  icon?: string
  label: string
  value: number
  suffix?: string
  color?: string
}>(), {
  icon: '📊',
  suffix: '',
  color: '#1B3A5C',
})

const displayed = ref(0)
const isVisible = ref(false)

function animateCount(target: number) {
  const duration = 1800
  const start = performance.now()
  const startVal = displayed.value

  function step(now: number) {
    const elapsed = now - start
    const progress = Math.min(elapsed / duration, 1)
    // Ease out quart
    const eased = 1 - Math.pow(1 - progress, 4)
    displayed.value = startVal + (target - startVal) * eased
    if (progress < 1) requestAnimationFrame(step)
  }
  requestAnimationFrame(step)
}

onMounted(() => {
  // Use IntersectionObserver to trigger count-up when visible
  const observer = new IntersectionObserver(
    (entries) => {
      if (entries[0].isIntersecting && !isVisible.value) {
        isVisible.value = true
        animateCount(props.value)
      }
    },
    { threshold: 0.3 }
  )

  // Observe parent after a short delay to ensure DOM is ready
  setTimeout(() => {
    const el = document.querySelector(`[data-metric="${props.label}"]`)
    if (el) observer.observe(el)
  }, 100)
})

watch(() => props.value, (newVal) => {
  if (isVisible.value) animateCount(newVal)
})

const formattedValue = (v: number): string => {
  if (props.suffix === '%') return v.toFixed(1)
  if (props.suffix === 'ms') return Math.round(v).toString()
  return v.toFixed(1)
}
</script>

<template>
  <div class="metric-card" :data-metric="label" :style="{ '--card-color': color }">
    <div class="metric-icon">{{ icon }}</div>
    <div class="metric-value">
      {{ formattedValue(displayed) }}<span class="metric-suffix">{{ suffix }}</span>
    </div>
    <div class="metric-label">{{ label }}</div>
  </div>
</template>

<style scoped>
.metric-card {
  background: #fff;
  border: 1px solid #E2E8F0;
  border-radius: 16px;
  padding: 28px 24px;
  text-align: center;
  box-shadow: 0 4px 16px rgba(27, 58, 92, 0.06);
  transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1), box-shadow 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  position: relative;
  overflow: hidden;
}
.metric-card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 4px;
  background: var(--card-color, #1B3A5C);
  border-radius: 16px 16px 0 0;
}
.metric-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 32px rgba(27, 58, 92, 0.12);
}
.metric-icon {
  font-size: 2rem;
  margin-bottom: 8px;
}
.metric-value {
  font-family: 'Playfair Display', serif;
  font-size: 2.8rem;
  font-weight: 700;
  color: var(--card-color, #1B3A5C);
  line-height: 1.1;
}
.metric-suffix {
  font-size: 1.4rem;
  font-weight: 400;
  margin-left: 2px;
  opacity: 0.7;
}
.metric-label {
  margin-top: 8px;
  font-family: 'Inter', sans-serif;
  font-size: 0.85rem;
  color: #6C7A89;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}
</style>
