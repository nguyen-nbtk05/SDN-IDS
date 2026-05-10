<!--
  FILE: components/SdnTopology.vue
  DESC: SDN topology — CSS-isolated for Slidev
-->
<template>
  <div class="sdn-topology" :style="{ maxWidth: width + 'px' }">
    <svg :viewBox="`0 0 ${vw} ${vh}`" width="100%" preserveAspectRatio="xMidYMid meet"
         style="font-size: 0; overflow: visible;">
      <defs>
        <linearGradient id="linkGrad3" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop offset="0%" stop-color="#4f46e5" stop-opacity="0.5"/>
          <stop offset="100%" stop-color="#0891b2" stop-opacity="0.5"/>
        </linearGradient>
      </defs>

      <!-- Control Plane zone -->
      <rect :x="vw*0.1" y="5" :width="vw*0.55" :height="vh*0.28" rx="6"
        fill="rgba(79,70,229,0.05)" stroke="rgba(79,70,229,0.15)" stroke-width="0.5" stroke-dasharray="3,2"/>
      <text :x="vw*0.375" y="14" text-anchor="middle"
        style="font-size:5px;fill:#4f46e5;font-weight:600;letter-spacing:0.08em">CONTROL PLANE</text>

      <!-- Data Plane zone -->
      <rect :x="vw*0.05" :y="vh*0.38" :width="vw*0.65" :height="vh*0.58" rx="6"
        fill="rgba(8,145,178,0.03)" stroke="rgba(8,145,178,0.12)" stroke-width="0.5" stroke-dasharray="3,2"/>
      <text :x="vw*0.375" :y="vh*0.42+5" text-anchor="middle"
        style="font-size:5px;fill:#0891b2;font-weight:600;letter-spacing:0.08em">DATA PLANE</text>

      <!-- OpenFlow connections -->
      <line v-for="(s,i) in sws" :key="'of'+i"
        :x1="ctrlX" :y1="ctrlY+8" :x2="s.x" :y2="s.y-8"
        stroke="url(#linkGrad3)" stroke-width="0.6" stroke-dasharray="2,2" opacity="0.6"/>

      <!-- Host connections -->
      <line v-for="(l,i) in hLinks" :key="'hl'+i"
        :x1="l.sx" :y1="l.sy+7" :x2="l.hx" :y2="l.hy-6"
        stroke="rgba(0,0,0,0.08)" stroke-width="0.4"/>

      <!-- Controller -->
      <rect :x="ctrlX-18" :y="ctrlY-8" width="36" height="16" rx="4"
        fill="rgba(79,70,229,0.1)" stroke="#4f46e5" stroke-width="0.8"/>
      <text :x="ctrlX" :y="ctrlY+2" text-anchor="middle"
        style="font-size:4.5px;fill:#4f46e5;font-weight:600">Controller</text>

      <!-- Switches -->
      <g v-for="(s,i) in sws" :key="'sw'+i">
        <rect :x="s.x-14" :y="s.y-7" width="28" height="14" rx="3"
          fill="rgba(8,145,178,0.08)" stroke="#0891b2" stroke-width="0.6"/>
        <text :x="s.x" :y="s.y+2" text-anchor="middle"
          style="font-size:4px;fill:#0891b2;font-weight:600">{{s.label}}</text>
      </g>

      <!-- Hosts -->
      <g v-for="(h,i) in hosts" :key="'h'+i">
        <circle :cx="h.x" :cy="h.y" r="5"
          fill="rgba(5,150,105,0.08)" stroke="#059669" stroke-width="0.5"/>
        <text :x="h.x" :y="h.y+1.8" text-anchor="middle"
          style="font-size:3.5px;fill:#059669;font-weight:600">{{h.label}}</text>
      </g>

      <!-- IDS Module -->
      <rect :x="vw*0.72" :y="ctrlY-12" width="40" height="24" rx="4"
        fill="rgba(220,38,38,0.06)" stroke="#dc2626" stroke-width="0.5" stroke-dasharray="2,1"/>
      <text :x="vw*0.72+20" :y="ctrlY-2" text-anchor="middle"
        style="font-size:4px;fill:#dc2626;font-weight:600">IDS Module</text>
      <text :x="vw*0.72+20" :y="ctrlY+5" text-anchor="middle"
        style="font-size:3.5px;fill:#dc2626;opacity:0.7">Entropy</text>

      <!-- IDS line -->
      <line :x1="ctrlX+19" :y1="ctrlY" :x2="vw*0.72" :y2="ctrlY"
        stroke="#dc2626" stroke-width="0.5" stroke-dasharray="2,1" opacity="0.4"/>

      <!-- OpenFlow label -->
      <rect :x="ctrlX+22" :y="(ctrlY+sws[0].y)/2-4" width="28" height="8" rx="2"
        fill="rgba(79,70,229,0.06)" stroke="rgba(79,70,229,0.15)" stroke-width="0.3"/>
      <text :x="ctrlX+36" :y="(ctrlY+sws[0].y)/2+1.5" text-anchor="middle"
        style="font-size:3.2px;fill:#4f46e5;font-weight:500">OpenFlow</text>
    </svg>
  </div>
</template>
<script setup>
import { computed } from 'vue'
const props = defineProps({ width: { type: Number, default: 480 } })
const vw = 260
const vh = 160
const ctrlX = vw * 0.375
const ctrlY = vh * 0.18
const sws = [
  { x: vw * 0.18, y: vh * 0.52, label: 'SW1' },
  { x: vw * 0.375, y: vh * 0.52, label: 'SW2' },
  { x: vw * 0.56, y: vh * 0.52, label: 'SW3' }
]
const hosts = [
  { x: vw*0.1, y: vh*0.78, label:'H1', sw:0 },
  { x: vw*0.22, y: vh*0.82, label:'H2', sw:0 },
  { x: vw*0.3, y: vh*0.78, label:'H3', sw:1 },
  { x: vw*0.44, y: vh*0.82, label:'H4', sw:1 },
  { x: vw*0.5, y: vh*0.78, label:'H5', sw:2 },
  { x: vw*0.62, y: vh*0.82, label:'H6', sw:2 },
]
const hLinks = computed(() => hosts.map(ho => ({
  sx: sws[ho.sw].x, sy: sws[ho.sw].y, hx: ho.x, hy: ho.y
})))
</script>
<style scoped>
.sdn-topology { display: flex; justify-content: center; width: 100%; margin: 0 auto; }
.sdn-topology svg { display: block; font-size: 0 !important; }
.sdn-topology svg text { font-size: inherit; }
</style>
