<!--
  FILE: components/TimelineStep.vue
  DESC: Timeline step — warm white theme
-->
<template>
  <div class="timeline-step" :class="{ active }">
    <div class="timeline-marker">
      <div class="marker-dot"></div>
      <div class="marker-line" v-if="!last"></div>
    </div>
    <div class="timeline-body">
      <div class="timeline-title">{{ title }}</div>
      <div class="timeline-desc"><slot /></div>
    </div>
  </div>
</template>
<script setup>
defineProps({
  title: { type: String, default: '' },
  active: { type: Boolean, default: false },
  last: { type: Boolean, default: false }
})
</script>
<style scoped>
.timeline-step { display: flex; gap: 14px; }
.timeline-marker { display: flex; flex-direction: column; align-items: center; flex-shrink: 0; }
.marker-dot {
  width: 12px; height: 12px; border-radius: 50%;
  background: rgba(79,70,229,0.12); border: 2px solid #4f46e5;
  transition: all 0.3s ease;
}
.active .marker-dot { background: #4f46e5; box-shadow: 0 0 8px rgba(79,70,229,0.25); }
.marker-line { width: 2px; flex: 1; min-height: 20px; background: rgba(79,70,229,0.12); margin-top: 4px; }
.timeline-title { font-size: 0.82rem; font-weight: 700; color: var(--sdn-text-primary); margin-bottom: 2px; }
.timeline-desc { font-size: 0.76rem; color: var(--sdn-text-muted); padding-bottom: 12px; line-height: 1.5; }
.timeline-desc :deep(p) { font-size: 0.76rem; margin: 2px 0; }
</style>
