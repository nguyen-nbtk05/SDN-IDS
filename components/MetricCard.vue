<!--
  FILE: components/MetricCard.vue
  DESC: Metric card — warm white theme
-->
<template>
  <div class="metric-card glass-card" :class="[`variant-${variant}`]">
    <div class="metric-icon" v-if="icon"><span class="icon-text">{{ icon }}</span></div>
    <div class="metric-body">
      <div class="metric-val">{{ value }}</div>
      <div class="metric-lbl">{{ label }}</div>
      <div class="metric-desc" v-if="description">{{ description }}</div>
    </div>
  </div>
</template>
<script setup>
defineProps({
  icon: { type: String, default: '' },
  value: { type: String, default: '0' },
  label: { type: String, default: 'Metric' },
  description: { type: String, default: '' },
  variant: { type: String, default: 'primary' }
})
</script>
<style scoped>
.metric-card {
  padding: 16px 18px;
  display: flex; gap: 12px;
  align-items: flex-start;
  min-width: 150px;
}
.metric-icon {
  width: 38px; height: 38px; border-radius: 10px;
  display: flex; align-items: center; justify-content: center;
  font-size: 1.1rem; flex-shrink: 0;
}
.variant-primary .metric-icon { background: rgba(79,70,229,0.1); }
.variant-accent .metric-icon { background: rgba(8,145,178,0.1); }
.variant-success .metric-icon { background: rgba(5,150,105,0.1); }
.variant-warning .metric-icon { background: rgba(217,119,6,0.1); }
.variant-danger .metric-icon { background: rgba(220,38,38,0.1); }
.metric-val { font-size: 1.5rem; font-weight: 800; line-height: 1; }
.variant-primary .metric-val { color: #4f46e5; }
.variant-accent .metric-val { color: #0891b2; }
.variant-success .metric-val { color: #059669; }
.variant-warning .metric-val { color: #d97706; }
.variant-danger .metric-val { color: #dc2626; }
.metric-lbl {
  font-size: 0.72rem; font-weight: 600; text-transform: uppercase;
  letter-spacing: 0.06em; color: var(--sdn-text-muted); margin-top: 4px;
}
.metric-desc { font-size: 0.7rem; color: var(--sdn-text-muted); margin-top: 2px; line-height: 1.4; }
</style>
