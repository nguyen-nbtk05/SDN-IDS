<template>
  <div class="flow-stats" :style="{ maxWidth: width + 'px' }">
    <svg :viewBox="`0 0 ${vw} ${vh}`" width="100%" preserveAspectRatio="xMidYMid meet" style="font-size: 0; overflow: visible;">
      <line
        v-for="i in 4"
        :key="'g' + i"
        :x1="pad"
        :y1="pad + (chartH / 4) * (i - 1)"
        :x2="vw - pad"
        :y2="pad + (chartH / 4) * (i - 1)"
        stroke="#e2e8f0"
        stroke-width="0.35"
      />
      <line :x1="pad" :y1="pad" :x2="pad" :y2="vh - pad" stroke="#cbd5e1" stroke-width="0.45" />
      <line :x1="pad" :y1="vh - pad" :x2="vw - pad" :y2="vh - pad" stroke="#cbd5e1" stroke-width="0.45" />

      <g v-for="(bar, i) in bars" :key="'b' + i">
        <rect
          :x="bar.x"
          :y="bar.y"
          :width="barW"
          :height="bar.h"
          rx="2"
          :fill="bar.color"
          opacity="0.82"
          class="bar-rect"
          :style="{ animationDelay: i * 75 + 'ms' }"
        />
        <text :x="bar.x + barW / 2" :y="vh - pad + 5" text-anchor="middle" style="font-size:3.2px;fill:#64748b;font-weight:700">
          {{ bar.label }}
        </text>
        <text :x="bar.x + barW / 2" :y="bar.y - 2" text-anchor="middle" :style="`font-size:3px;fill:${bar.color};font-weight:850`">
          {{ bar.val }}
        </text>
      </g>

      <text :x="vw / 2" :y="vh - 1" text-anchor="middle" style="font-size:3.5px;fill:#94a3b8;font-weight:650">
        Flow entry statistics
      </text>
    </svg>
  </div>
</template>

<script setup>
const props = defineProps({ width: { type: Number, default: 500 } })
const vw = 200
const vh = 100
const pad = 20
const chartH = vh - pad * 2
const data = [
  { label: 'TCP', val: 2450, color: '#2563eb' },
  { label: 'UDP', val: 1830, color: '#0891b2' },
  { label: 'ICMP', val: 420, color: '#16a34a' },
  { label: 'SYN', val: 3200, color: '#dc2626' },
  { label: 'ACK', val: 1650, color: '#d97706' },
  { label: 'DNS', val: 780, color: '#7c3aed' },
]
const maxVal = Math.max(...data.map((d) => d.val))
const gap = 4
const barW = (vw - pad * 2 - gap * (data.length - 1)) / data.length
const bars = data.map((d, i) => {
  const bh = (d.val / maxVal) * chartH * 0.85
  return { ...d, x: pad + i * (barW + gap), y: pad + chartH - bh, h: bh }
})
</script>

<style scoped>
.flow-stats {
  display: flex;
  width: 100%;
  justify-content: center;
  margin: 0 auto;
}

.flow-stats svg {
  display: block;
  font-size: 0 !important;
}

.bar-rect {
  animation: growBar 0.62s var(--sdn-ease-smooth) both;
  transform-origin: bottom;
}

@keyframes growBar {
  from {
    transform: scaleY(0);
  }
  to {
    transform: scaleY(1);
  }
}
</style>
