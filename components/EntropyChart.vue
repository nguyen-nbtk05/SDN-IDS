<!--
  FILE: components/EntropyChart.vue
  DESC: Entropy chart — CSS-isolated for Slidev
-->
<template>
  <div class="entropy-chart" :style="{ maxWidth: width + 'px' }">
    <svg :viewBox="`0 0 ${vw} ${vh}`" width="100%" preserveAspectRatio="xMidYMid meet"
         style="font-size: 0; overflow: visible;">
      <defs>
        <linearGradient id="ec-area" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#4f46e5" stop-opacity="0.12"/>
          <stop offset="100%" stop-color="#4f46e5" stop-opacity="0.01"/>
        </linearGradient>
        <linearGradient id="ec-atk" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#dc2626" stop-opacity="0.12"/>
          <stop offset="100%" stop-color="#dc2626" stop-opacity="0.01"/>
        </linearGradient>
      </defs>

      <!-- Grid -->
      <line v-for="i in 4" :key="'gy'+i"
        :x1="pad" :y1="pad+(chartH/4)*(i-1)" :x2="vw-pad" :y2="pad+(chartH/4)*(i-1)"
        stroke="rgba(0,0,0,0.05)" stroke-width="0.3"/>
      <!-- Axes -->
      <line :x1="pad" :y1="pad" :x2="pad" :y2="vh-pad" stroke="rgba(0,0,0,0.15)" stroke-width="0.4"/>
      <line :x1="pad" :y1="vh-pad" :x2="vw-pad" :y2="vh-pad" stroke="rgba(0,0,0,0.15)" stroke-width="0.4"/>

      <!-- Y labels -->
      <text v-for="(lbl,i) in yLabels" :key="'yl'+i"
        :x="pad-3" :y="pad+chartH-chartH*lbl.val/maxY+1.5"
        text-anchor="end" style="font-size:3.5px;fill:#a8a29e">{{lbl.text}}</text>

      <!-- Threshold -->
      <line :x1="pad" :y1="thresholdY" :x2="vw-pad" :y2="thresholdY"
        stroke="#d97706" stroke-width="0.5" stroke-dasharray="2,1.5" opacity="0.7"/>
      <text :x="vw-pad+2" :y="thresholdY+1.2"
        style="font-size:3px;fill:#d97706;font-weight:600">Threshold</text>

      <!-- Area fills -->
      <path :d="areaPath" fill="url(#ec-area)"/>
      <path :d="attackAreaPath" fill="url(#ec-atk)" v-if="attackAreaPath"/>

      <!-- Lines -->
      <path :d="linePath" fill="none" stroke="#4f46e5" stroke-width="0.8"
        stroke-linejoin="round" stroke-linecap="round" class="entropy-line"/>
      <path :d="attackLinePath" fill="none" stroke="#dc2626" stroke-width="0.8"
        stroke-linejoin="round" stroke-linecap="round" v-if="attackLinePath"/>

      <!-- Points -->
      <circle v-for="(pt,i) in points" :key="'p'+i"
        :cx="pt.x" :cy="pt.y" r="1.2"
        :fill="pt.attack?'#dc2626':'#4f46e5'"
        :stroke="pt.attack?'#fecaca':'#c7d2fe'" stroke-width="0.5"/>

      <!-- X label -->
      <text :x="vw/2" :y="vh-1" text-anchor="middle"
        style="font-size:3.5px;fill:#a8a29e">Thời gian (chu kỳ 5s)</text>
      <!-- Y label -->
      <text :x="4" :y="vh/2" text-anchor="middle"
        style="font-size:3.5px;fill:#a8a29e"
        :transform="`rotate(-90,4,${vh/2})`">Entropy (H)</text>

      <!-- Attack annotation -->
      <g v-if="attackStart>=0">
        <rect :x="points[attackStart].x-1" :y="3" width="26" height="8" rx="2"
          fill="rgba(220,38,38,0.08)" stroke="rgba(220,38,38,0.2)" stroke-width="0.3"/>
        <text :x="points[attackStart].x+12" y="8.5" text-anchor="middle"
          style="font-size:3px;fill:#dc2626;font-weight:600">⚠ DDoS Attack</text>
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
const attackStart = data.findIndex(v => v < threshold)
const points = computed(() => data.map((v, i) => ({
  x: pad + (i / (data.length - 1)) * chartW,
  y: pad + chartH - (v / maxY) * chartH,
  val: v, attack: v < threshold
})))
const thresholdY = pad + chartH - (threshold / maxY) * chartH
const yLabels = [{ val: 0, text: '0.0' },{ val: 1.0, text: '1.0' },{ val: 2.0, text: '2.0' },{ val: 3.0, text: '3.0' }]
function buildPath(pts) { return pts.map((p, i) => `${i===0?'M':'L'}${p.x},${p.y}`).join(' ') }
const linePath = computed(() => buildPath(points.value.filter(p => !p.attack)))
const attackLinePath = computed(() => {
  const si = data.findIndex(v => v < threshold)
  const ei = data.length - 1 - [...data].reverse().findIndex(v => v < threshold)
  if (si < 0) return ''; return buildPath(points.value.slice(Math.max(0, si - 1), ei + 2))
})
const areaPath = computed(() => {
  const n = points.value.filter(p => !p.attack); if (!n.length) return ''
  const b = pad + chartH; return buildPath(n) + ` L${n[n.length-1].x},${b} L${n[0].x},${b} Z`
})
const attackAreaPath = computed(() => {
  const si = data.findIndex(v => v < threshold)
  const ei = data.length - 1 - [...data].reverse().findIndex(v => v < threshold)
  if (si < 0) return ''; const sl = points.value.slice(Math.max(0, si - 1), ei + 2)
  if (!sl.length) return ''; const b = pad + chartH
  return buildPath(sl) + ` L${sl[sl.length-1].x},${b} L${sl[0].x},${b} Z`
})
</script>
<style scoped>
.entropy-chart { display: flex; justify-content: center; width: 100%; margin: 0 auto; }
.entropy-chart svg { display: block; font-size: 0 !important; }
.entropy-line { stroke-dasharray: 500; stroke-dashoffset: 500; animation: drawLine 2s ease forwards; }
@keyframes drawLine { to { stroke-dashoffset: 0; } }
</style>
