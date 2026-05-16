<template>
  <div
    v-motion
    class="sdn-topology"
    :style="{ maxWidth: width + 'px' }"
    :initial="{ opacity: 0, y: 12 }"
    :enter="{ opacity: 1, y: 0, transition: { duration: 520, ease: 'easeOut' } }"
  >
    <svg :viewBox="`0 0 ${vw} ${vh}`" width="100%" preserveAspectRatio="xMidYMid meet">
      <defs>
        <marker id="topology-arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="3.5" markerHeight="3.5" orient="auto-start-reverse">
          <path d="M 0 0 L 10 5 L 0 10 z" fill="#2563eb" />
        </marker>
      </defs>

      <rect x="8" y="8" width="304" height="174" rx="14" class="topology-frame" />
      <text x="160" y="24" text-anchor="middle" class="zone-label">MININET STAR TOPOLOGY · 1 SWITCH · 16 HOSTS</text>

      <g class="links">
        <line class="control-link topology-link" :x1="ctrl.x" :y1="ctrl.y + 18" :x2="sw.x" :y2="sw.y - 18" marker-end="url(#topology-arrow)" />
        <line
          v-for="(h, i) in hosts"
          :key="h.label"
          class="host-link topology-link"
          :class="h.role"
          :x1="sw.x"
          :y1="sw.y + 18"
          :x2="h.x"
          :y2="h.y - 10"
          :style="{ animationDelay: `${180 + i * 28}ms` }"
        />
      </g>

      <g class="device controller-node" :transform="`translate(${ctrl.x}, ${ctrl.y})`">
        <rect x="-34" y="-18" width="68" height="36" rx="10" />
        <foreignObject x="-10" y="-14" width="20" height="20"><span class="device-icon i-twemoji-antenna-bars"></span></foreignObject>
        <text y="15" text-anchor="middle">C0 · 127.0.0.1:6633</text>
      </g>

      <g class="device switch-node" :transform="`translate(${sw.x}, ${sw.y})`">
        <rect x="-30" y="-16" width="60" height="32" rx="9" />
        <path d="M -16 -3 H 16 M -10 -8 H 10 M -10 8 H 10" />
        <text y="26" text-anchor="middle">S1 · OF1.3</text>
      </g>

      <g v-for="h in hosts" :key="h.label" class="device host-node" :class="h.role" :transform="`translate(${h.x}, ${h.y})`">
        <rect x="-12" y="-9" width="24" height="18" rx="4" />
        <text y="20" text-anchor="middle">{{ h.label }}</text>
      </g>

      <g class="legend">
        <rect x="205" y="154" width="82" height="17" rx="8.5" />
        <circle cx="216" cy="162.5" r="3" class="victim-dot" />
        <text x="224" y="165">Victim / Benign / Attacker</text>
      </g>
    </svg>
  </div>
</template>

<script setup>
defineProps({ width: { type: Number, default: 560 } })

const vw = 320
const vh = 190
const ctrl = { x: 160, y: 48 }
const sw = { x: 160, y: 96 }
const hosts = [
  { label: 'h_vic', role: 'victim', x: 160, y: 145 },
  ...Array.from({ length: 5 }, (_, i) => ({
    label: `h_ben${i + 1}`,
    role: 'benign',
    x: 58 + i * 24,
    y: 145 + (i % 2) * 18,
  })),
  ...Array.from({ length: 10 }, (_, i) => ({
    label: `h_atk${i + 1}`,
    role: 'attacker',
    x: 196 + (i % 5) * 24,
    y: 145 + Math.floor(i / 5) * 18,
  })),
]
</script>

<style scoped>
.sdn-topology {
  width: 100%;
  margin: 0 auto;
}

.sdn-topology svg {
  display: block;
  overflow: visible;
  font-size: 0 !important;
}

.topology-frame {
  fill: #ffffff;
  stroke: #dbe5f0;
  stroke-width: 1;
}

.zone-label {
  fill: #64748b;
  font-size: 5px;
  font-weight: 850;
  letter-spacing: 0.05em;
}

.topology-link {
  fill: none;
  stroke-linecap: round;
  stroke-dasharray: 120;
  stroke-dashoffset: 120;
  animation: drawLine 1.05s var(--sdn-ease-smooth) forwards;
}

.control-link {
  stroke: #2563eb;
  stroke-width: 1.2;
}

.host-link {
  stroke: #94a3b8;
  stroke-width: 0.65;
}

.host-link.victim {
  stroke: #16a34a;
}

.host-link.attacker {
  stroke: #dc2626;
}

.device {
  animation: scaleIn 0.55s var(--sdn-ease-smooth) both;
  transform-box: fill-box;
  transform-origin: center;
}

.device rect {
  fill: #ffffff;
  stroke-width: 1;
}

.device text {
  fill: #334155;
  font-size: 4.2px;
  font-weight: 800;
}

.device-icon {
  display: inline-block;
  width: 20px;
  height: 20px;
}

.controller-node rect {
  fill: #eff6ff;
  stroke: #2563eb;
}

.controller-node .device-icon {
  color: #2563eb;
}

.switch-node rect {
  fill: #f8fafc;
  stroke: #64748b;
}

.switch-node path {
  fill: none;
  stroke: #64748b;
  stroke-linecap: round;
  stroke-width: 1;
}

.host-node rect {
  fill: #ffffff;
  stroke: #64748b;
}

.host-node.victim rect {
  fill: #f0fdf4;
  stroke: #16a34a;
}

.host-node.attacker rect {
  fill: #fef2f2;
  stroke: #dc2626;
}

.legend rect {
  fill: #ffffff;
  stroke: #e2e8f0;
}

.legend text {
  fill: #64748b;
  font-size: 4px;
  font-weight: 760;
}

.victim-dot {
  fill: #16a34a;
}
</style>
