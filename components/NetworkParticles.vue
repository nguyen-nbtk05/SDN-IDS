<!--
  FILE: components/NetworkParticles.vue
  DESC: Animated network particles — warm white theme (soft muted colors)
-->
<template>
  <div class="network-particles" ref="container">
    <svg :width="width" :height="height" class="particles-svg">
      <line v-for="(conn, idx) in connections" :key="'c-' + idx"
        :x1="conn.x1" :y1="conn.y1" :x2="conn.x2" :y2="conn.y2"
        :stroke="conn.color" :stroke-width="conn.width" :opacity="conn.opacity"
        stroke-linecap="round" class="connection-line"/>
      <circle v-for="(flow, idx) in dataFlows" :key="'f-' + idx"
        :cx="flow.cx" :cy="flow.cy" :r="flow.r" :fill="flow.color" :opacity="flow.opacity"
        class="flow-particle"/>
      <g v-for="(node, idx) in nodes" :key="'n-' + idx">
        <circle :cx="node.x" :cy="node.y" :r="node.r + 4"
          :fill="node.glowColor" :opacity="node.glowOpacity" class="node-glow"/>
        <circle :cx="node.x" :cy="node.y" :r="node.r"
          :fill="node.color" :opacity="node.opacity" class="node-core"/>
      </g>
    </svg>
  </div>
</template>
<script setup>
import { ref, reactive, onMounted, onUnmounted } from 'vue';
const props = defineProps({
  count: { type: Number, default: 25 },
  speed: { type: Number, default: 0.3 },
  connectionDistance: { type: Number, default: 150 },
  colors: { type: Array, default: () => ['#a5b4fc', '#99f6e4', '#c4b5fd', '#bae6fd', '#bbf7d0'] }
});
const container = ref(null);
const width = ref(960);
const height = ref(540);
let animationId = null;
const nodes = reactive([]);
const connections = reactive([]);
const dataFlows = reactive([]);
function createNodes() {
  nodes.length = 0;
  for (let i = 0; i < props.count; i++) {
    nodes.push({
      x: Math.random() * width.value, y: Math.random() * height.value,
      vx: (Math.random() - 0.5) * props.speed, vy: (Math.random() - 0.5) * props.speed,
      r: 2 + Math.random() * 2.5,
      color: props.colors[Math.floor(Math.random() * props.colors.length)],
      glowColor: props.colors[Math.floor(Math.random() * props.colors.length)],
      opacity: 0.3 + Math.random() * 0.4,
      glowOpacity: 0.05 + Math.random() * 0.08,
      phase: Math.random() * Math.PI * 2
    });
  }
}
function updateNodes(time) {
  for (const node of nodes) {
    node.x += node.vx; node.y += node.vy;
    if (node.x < 0 || node.x > width.value) node.vx *= -1;
    if (node.y < 0 || node.y > height.value) node.vy *= -1;
    node.opacity = 0.25 + Math.sin(time * 0.001 + node.phase) * 0.2;
    node.glowOpacity = 0.04 + Math.sin(time * 0.002 + node.phase) * 0.04;
  }
}
function updateConnections() {
  connections.length = 0;
  for (let i = 0; i < nodes.length; i++) {
    for (let j = i + 1; j < nodes.length; j++) {
      const dx = nodes[i].x - nodes[j].x, dy = nodes[i].y - nodes[j].y;
      const dist = Math.sqrt(dx * dx + dy * dy);
      if (dist < props.connectionDistance) {
        connections.push({
          x1: nodes[i].x, y1: nodes[i].y, x2: nodes[j].x, y2: nodes[j].y,
          color: nodes[i].color, width: 0.4 + (1 - dist / props.connectionDistance) * 0.5,
          opacity: (1 - dist / props.connectionDistance) * 0.15
        });
      }
    }
  }
}
function updateDataFlows(time) {
  dataFlows.length = 0;
  const maxFlows = Math.min(connections.length, 6);
  for (let i = 0; i < maxFlows; i++) {
    const conn = connections[i]; if (!conn) continue;
    const t = ((time * 0.001 + i * 0.7) % 1);
    dataFlows.push({
      cx: conn.x1 + (conn.x2 - conn.x1) * t,
      cy: conn.y1 + (conn.y2 - conn.y1) * t,
      r: 1.2, color: '#a5b4fc', opacity: 0.35 * Math.sin(t * Math.PI)
    });
  }
}
function animate(time) { updateNodes(time); updateConnections(); updateDataFlows(time); animationId = requestAnimationFrame(animate); }
onMounted(() => {
  if (container.value) { width.value = container.value.clientWidth || 960; height.value = container.value.clientHeight || 540; }
  createNodes(); animationId = requestAnimationFrame(animate);
});
onUnmounted(() => { if (animationId) cancelAnimationFrame(animationId); });
</script>
<style scoped>
.network-particles { position: absolute; inset: 0; pointer-events: none; z-index: 0; overflow: hidden; }
.particles-svg { width: 100%; height: 100%; }
</style>
