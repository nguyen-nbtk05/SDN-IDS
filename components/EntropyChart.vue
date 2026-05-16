<template>
  <div class="entropy-chart" :style="{ maxWidth: width + 'px' }">
    <svg :viewBox="`0 0 ${vw} ${vh}`" width="100%" preserveAspectRatio="xMidYMid meet" style="font-size: 0; overflow: visible;">
      <defs>
        <linearGradient id="entropy-area-normal" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#2563eb" stop-opacity="0.14" />
          <stop offset="100%" stop-color="#2563eb" stop-opacity="0.02" />
        </linearGradient>
        <linearGradient id="entropy-area-attack" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#dc2626" stop-opacity="0.14" />
          <stop offset="100%" stop-color="#dc2626" stop-opacity="0.02" />
        </linearGradient>
      </defs>

      <line
        v-for="i in 4"
        :key="'gy' + i"
        :x1="pad"
        :y1="pad + (chartH / 4) * (i - 1)"
        :x2="vw - pad"
        :y2="pad + (chartH / 4) * (i - 1)"
        stroke="#e2e8f0"
        stroke-width="0.35"
      />
      <line :x1="pad" :y1="pad" :x2="pad" :y2="vh - pad" stroke="#cbd5e1" stroke-width="0.45" />
      <line :x1="pad" :y1="vh - pad" :x2="vw - pad" :y2="vh - pad" stroke="#cbd5e1" stroke-width="0.45" />

      <text
        v-for="(lbl, i) in yLabels"
        :key="'yl' + i"
        :x="pad - 3"
        :y="pad + chartH - (chartH * lbl.val) / maxY + 1.5"
        text-anchor="end"
        style="font-size:3.5px;fill:#94a3b8;font-weight:650"
      >
        {{ lbl.text }}
      </text>

      <line :x1="pad" :y1="thresholdY" :x2="vw - pad" :y2="thresholdY" stroke="#d97706" stroke-width="0.55" stroke-dasharray="2,1.5" />
      <text :x="vw - pad + 2" :y="thresholdY + 1.2" style="font-size:3px;fill:#d97706;font-weight:760">threshold</text>

      <path :d="areaPath" fill="url(#entropy-area-normal)" />
      <path :d="attackAreaPath" fill="url(#entropy-area-attack)" v-if="attackAreaPath" />

      <path :d="linePath" fill="none" stroke="#2563eb" stroke-width="0.9" stroke-linejoin="round" stroke-linecap="round" class="entropy-line" />
      <path :d="attackLinePath" fill="none" stroke="#dc2626" stroke-width="0.9" stroke-linejoin="round" stroke-linecap="round" v-if="attackLinePath" />

      <circle
        v-for="(pt, i) in points"
        :key="'p' + i"
        :cx="pt.x"
        :cy="pt.y"
        r="1.15"
        :fill="pt.attack ? '#dc2626' : '#2563eb'"
        :stroke="pt.attack ? '#fee2e2' : '#dbeafe'"
        stroke-width="0.55"
      />

      <text :x="vw / 2" :y="vh - 1" text-anchor="middle" style="font-size:3.5px;fill:#94a3b8;font-weight:650">Time window (5s)</text>
      <text :x="4" :y="vh / 2" text-anchor="middle" style="font-size:3.5px;fill:#94a3b8;font-weight:650" :transform="`rotate(-90,4,${vh / 2})`">
        Entropy H
      </text>

      <g v-if="attackStart >= 0">
        <rect :x="points[attackStart].x - 1" y="3" width="29" height="8" rx="2.5" fill="#fef2f2" stroke="#fecaca" stroke-width="0.35" />
        <text :x="points[attackStart].x + 13.5" y="8.5" text-anchor="middle" style="font-size:3px;fill:#dc2626;font-weight:760">
          anomaly zone
        </text>
      </g>
    </svg>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({ width: { type: Number, default: 700 } })
const vw = 230
const vh = 120
const pad = 22
const chartW = vw - pad * 2
const chartH = vh - pad * 2
const threshold = 1.0
const maxY = 3.5
const data = [3.2, 3.1, 3.0, 2.9, 3.1, 3.0, 2.8, 2.5, 1.8, 0.9, 0.5, 0.3, 0.4, 0.8, 1.5, 2.2, 2.8, 3.0]
const attackStart = data.findIndex((v) => v < threshold)
const points = computed(() =>
  data.map((v, i) => ({
    x: pad + (i / (data.length - 1)) * chartW,
    y: pad + chartH - (v / maxY) * chartH,
    val: v,
    attack: v < threshold,
  })),
)
const thresholdY = pad + chartH - (threshold / maxY) * chartH
const yLabels = [
  { val: 0, text: '0.0' },
  { val: 1.0, text: '1.0' },
  { val: 2.0, text: '2.0' },
  { val: 3.0, text: '3.0' },
]
function buildPath(pts) {
  return pts.map((p, i) => `${i === 0 ? 'M' : 'L'}${p.x},${p.y}`).join(' ')
}
const linePath = computed(() => buildPath(points.value.filter((p) => !p.attack)))
const attackLinePath = computed(() => {
  const si = data.findIndex((v) => v < threshold)
  const ei = data.length - 1 - [...data].reverse().findIndex((v) => v < threshold)
  if (si < 0) return ''
  return buildPath(points.value.slice(Math.max(0, si - 1), ei + 2))
})
const areaPath = computed(() => {
  const normal = points.value.filter((p) => !p.attack)
  if (!normal.length) return ''
  const base = pad + chartH
  return `${buildPath(normal)} L${normal[normal.length - 1].x},${base} L${normal[0].x},${base} Z`
})
const attackAreaPath = computed(() => {
  const si = data.findIndex((v) => v < threshold)
  const ei = data.length - 1 - [...data].reverse().findIndex((v) => v < threshold)
  if (si < 0) return ''
  const slice = points.value.slice(Math.max(0, si - 1), ei + 2)
  if (!slice.length) return ''
  const base = pad + chartH
  return `${buildPath(slice)} L${slice[slice.length - 1].x},${base} L${slice[0].x},${base} Z`
})
</script>

<style scoped>
.entropy-chart {
  display: flex;
  width: 100%;
  justify-content: center;
  margin: 0 auto;
}

.entropy-chart svg {
  display: block;
  font-size: 0 !important;
}

.entropy-line {
  animation: drawLine 1.8s ease forwards;
  stroke-dasharray: 500;
  stroke-dashoffset: 500;
}
</style>
