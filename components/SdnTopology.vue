<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

const props = withDefaults(defineProps<{
  highlightAttack?: boolean
}>(), { highlightAttack: false })

const tick = ref(0)
let intervalId: ReturnType<typeof setInterval>

onMounted(() => { intervalId = setInterval(() => { tick.value++ }, 100) })
onUnmounted(() => { clearInterval(intervalId) })

const attackerBlink = computed(() => props.highlightAttack ? Math.floor(tick.value / 4) % 2 === 0 : true)

const packets = computed(() => {
  if (!props.highlightAttack) return []
  return Array.from({ length: 5 }, (_, i) => ((tick.value * 3 + i * 20) % 100) / 100)
})

const CX = 370; const CY = 160
const switches = [{ x: 140 }, { x: 310 }, { x: 480 }, { x: 620 }]
const appNodes = [{ x: 200, l: 'Firewall' }, { x: 370, l: 'IDS Module' }, { x: 540, l: 'Load Balancer' }]
</script>

<template>
  <svg viewBox="0 0 740 410" class="sdn-topo" xmlns="http://www.w3.org/2000/svg">
    <defs>
      <filter id="rGlow" x="-50%" y="-50%" width="200%" height="200%">
        <feGaussianBlur stdDeviation="4" result="b"/><feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge>
      </filter>
      <marker id="arrN" viewBox="0 0 10 7" refX="10" refY="3.5" markerWidth="8" markerHeight="6" orient="auto-start-reverse">
        <polygon points="0 0,10 3.5,0 7" fill="#1B3A5C" opacity="0.5"/>
      </marker>
    </defs>

    <!-- Layer Backgrounds -->
    <rect x="20" y="20" width="700" height="65" rx="10" fill="#2A5580" fill-opacity="0.05" stroke="#2A5580" stroke-opacity="0.15"/>
    <rect x="20" y="120" width="700" height="80" rx="10" fill="#E8913A" fill-opacity="0.05" stroke="#E8913A" stroke-opacity="0.15"/>
    <rect x="20" y="240" width="700" height="70" rx="10" fill="#1B3A5C" fill-opacity="0.05" stroke="#1B3A5C" stroke-opacity="0.15"/>

    <!-- Layer Labels -->
    <text x="40" y="40" font-size="10" fill="#2A5580" opacity="0.6" font-family="Inter,sans-serif" font-weight="600">Application Layer</text>
    <text x="40" y="138" font-size="10" fill="#E8913A" opacity="0.6" font-family="Inter,sans-serif" font-weight="600">Control Layer</text>
    <text x="40" y="258" font-size="10" fill="#1B3A5C" opacity="0.6" font-family="Inter,sans-serif" font-weight="600">Data Layer</text>

    <!-- App→Controller lines -->
    <line v-for="(n, i) in appNodes" :key="'ac'+i" :x1="n.x" y1="68" :x2="CX" y2="148" stroke="#1B3A5C" stroke-opacity="0.2" stroke-width="1.5" stroke-dasharray="4,4" marker-end="url(#arrN)"/>

    <!-- Controller→Switch lines -->
    <line v-for="(s, i) in switches" :key="'cs'+i" :x1="CX" y1="178" :x2="s.x" y2="272" stroke="#1B3A5C" stroke-opacity="0.25" stroke-width="1.5" marker-end="url(#arrN)"/>

    <!-- App Nodes -->
    <g v-for="(n, i) in appNodes" :key="'a'+i">
      <rect :x="n.x-45" y="40" width="90" height="28" rx="6" fill="#fff" stroke="#2A5580" stroke-width="1.5"/>
      <text :x="n.x" y="58" text-anchor="middle" font-size="11" fill="#2A5580" font-family="Inter,sans-serif" font-weight="500">{{ n.l }}</text>
    </g>

    <!-- Controller -->
    <rect :x="CX-60" y="145" width="120" height="34" rx="8" fill="#E8913A" fill-opacity="0.12" stroke="#E8913A" stroke-width="2"/>
    <text :x="CX" y="166" text-anchor="middle" font-size="13" fill="#E8913A" font-family="Inter,sans-serif" font-weight="700">Ryu Controller</text>

    <!-- Switches -->
    <g v-for="(s, i) in switches" :key="'s'+i">
      <rect :x="s.x-35" y="268" width="70" height="26" rx="5" fill="#fff" stroke="#1B3A5C" stroke-width="1.5"/>
      <text :x="s.x" y="285" text-anchor="middle" font-size="11" fill="#1B3A5C" font-family="Inter,sans-serif" font-weight="500">OVS {{ i+1 }}</text>
      <line :x1="s.x" y1="298" :x2="s.x" y2="330" stroke="#1B3A5C" stroke-opacity="0.2"/>
      <circle :cx="s.x" :cy="338" r="8" fill="#FAFAF7" stroke="#1B3A5C" stroke-width="1.2"/>
      <text :x="s.x" :y="342" text-anchor="middle" font-size="7" fill="#1B3A5C">H{{ i+1 }}</text>
    </g>

    <!-- Attack Mode -->
    <template v-if="highlightAttack">
      <g :filter="attackerBlink ? 'url(#rGlow)' : 'none'">
        <line x1="80" y1="350" :x2="switches[0].x" y2="298" stroke="#DC3545" stroke-width="2" stroke-dasharray="6,3"/>
        <rect x="40" y="354" width="80" height="30" rx="6" :fill="attackerBlink?'#DC3545':'#fff'" stroke="#DC3545" stroke-width="2"/>
        <text x="80" y="373" text-anchor="middle" font-size="11" :fill="attackerBlink?'#fff':'#DC3545'" font-family="Inter,sans-serif" font-weight="700">⚠ Attacker</text>
      </g>
      <circle v-for="(p, pi) in packets" :key="'pk'+pi" :cx="switches[0].x+(CX-switches[0].x)*p" :cy="280+(CY-280+18)*p-18" r="3" fill="#DC3545" opacity="0.8"/>
      <rect :x="CX+80" y="150" width="130" height="24" rx="5" fill="#DC3545" fill-opacity="0.1" stroke="#DC3545"/>
      <text :x="CX+145" y="166" text-anchor="middle" font-size="10" fill="#DC3545" font-family="Inter,sans-serif" font-weight="600">🔴 DDoS Detected!</text>
    </template>
  </svg>
</template>

<style scoped>
.sdn-topo { width: 100%; max-width: 740px; height: auto; margin: 0 auto; display: block; }
</style>
