<!--
  FILE: layouts/two-cols-custom.vue
  PATH: /home/nora/Projects/slides/SDN-IDS/layouts/two-cols-custom.vue
  DESC: Two-column layout — warm white theme
-->
<template>
  <div class="slidev-layout two-cols-custom-layout">
    <div class="tc-bg"><div class="tc-mesh"></div></div>
    <div class="tc-header"><slot name="header" /></div>
    <div class="tc-columns">
      <div class="tc-left"><slot /></div>
      <div class="tc-divider"></div>
      <div class="tc-right"><slot name="right" /></div>
    </div>
  </div>
</template>
<style scoped>
.two-cols-custom-layout {
  position: relative;
  display: flex; flex-direction: column;
  padding: 40px 56px;
  overflow: hidden;
  background: var(--sdn-bg-deep);
}
.tc-bg { position: absolute; inset: 0; }
.tc-mesh {
  position: absolute; inset: 0;
  background:
    radial-gradient(ellipse 50% 40% at 0% 50%, rgba(79,70,229,0.03) 0%, transparent 70%),
    radial-gradient(ellipse 50% 40% at 100% 50%, rgba(8,145,178,0.02) 0%, transparent 70%);
}
.tc-header { position: relative; z-index: 1; margin-bottom: 20px; }
.tc-header :deep(h1) {
  font-size: 1.5rem; font-weight: 800;
  color: var(--sdn-text-primary);
  animation: fadeInUp 0.6s var(--sdn-ease-smooth) both;
}
.tc-columns {
  position: relative; z-index: 1;
  display: flex; gap: 28px; flex: 1; align-items: flex-start;
}
.tc-left, .tc-right { flex: 1; min-width: 0; }
.tc-left :deep(h2), .tc-right :deep(h2) {
  font-size: 1rem; font-weight: 700; color: var(--sdn-primary); margin-bottom: 8px;
}
.tc-left :deep(h3), .tc-right :deep(h3) {
  font-size: 0.9rem; font-weight: 600; color: var(--sdn-accent); margin-top: 10px; margin-bottom: 4px;
}
.tc-left :deep(p), .tc-right :deep(p) { font-size: 0.82rem; line-height: 1.6; }
.tc-left :deep(li), .tc-right :deep(li) { font-size: 0.82rem; margin-bottom: 4px; }
.tc-divider {
  width: 1px; align-self: stretch; flex-shrink: 0;
  background: linear-gradient(180deg, transparent, var(--sdn-glass-border), transparent);
}
</style>
