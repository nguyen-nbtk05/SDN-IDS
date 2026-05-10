<!--
  FILE: components/GlassBox.vue
  PATH: /home/nora/Projects/slides/SDN-IDS/components/GlassBox.vue
  DESC: Reusable glassmorphism container with optional title/icon
-->
<template>
  <div class="glass-box glass-card" :class="{ 'compact': compact }">
    <div class="glass-box-header" v-if="title">
      <span class="glass-box-icon" v-if="icon">{{ icon }}</span>
      <span class="glass-box-title">{{ title }}</span>
    </div>
    <div class="glass-box-content">
      <slot />
    </div>
  </div>
</template>
<script setup>
defineProps({
  title: { type: String, default: '' },
  icon: { type: String, default: '' },
  compact: { type: Boolean, default: false }
})
</script>
<style scoped>
.glass-box { padding: 18px 20px; }
.glass-box.compact { padding: 12px 16px; }
.glass-box-header {
  display: flex; align-items: center; gap: 8px;
  margin-bottom: 10px; padding-bottom: 8px;
  border-bottom: 1px solid var(--sdn-glass-border);
}
.glass-box-icon { font-size: 1rem; }
.glass-box-title {
  font-size: 0.82rem; font-weight: 700; color: var(--sdn-text-primary);
  text-transform: uppercase; letter-spacing: 0.04em;
}
.glass-box-content :deep(p) { font-size: 0.82rem; margin: 4px 0; }
.glass-box-content :deep(li) { font-size: 0.8rem; margin-bottom: 3px; }
</style>
