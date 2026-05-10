<!--
  FILE: layouts/content-card.vue
  PATH: /home/nora/Projects/slides/SDN-IDS/layouts/content-card.vue
  DESC: Content layout — warm white theme
-->
<template>
  <div class="slidev-layout content-card-layout">
    <div class="card-bg"><div class="card-mesh"></div></div>
    <div class="card-wrapper"><slot /></div>
  </div>
</template>
<style scoped>
.content-card-layout {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  padding: 40px 56px;
  overflow: hidden;
  background: var(--sdn-bg-deep);
}
.card-bg { position: absolute; inset: 0; }
.card-mesh {
  position: absolute; inset: 0;
  background:
    radial-gradient(ellipse 60% 40% at 10% 90%, rgba(79,70,229,0.03) 0%, transparent 70%),
    radial-gradient(ellipse 40% 30% at 90% 20%, rgba(8,145,178,0.02) 0%, transparent 70%);
}
.card-wrapper { position: relative; z-index: 1; width: 100%; }
.card-wrapper :deep(h1) {
  font-size: 1.65rem; font-weight: 800; margin-bottom: 6px;
  color: var(--sdn-text-primary);
  animation: fadeInUp 0.6s var(--sdn-ease-smooth) both;
}
.card-wrapper :deep(h2) {
  font-size: 1.1rem; font-weight: 600;
  color: var(--sdn-primary); margin-bottom: 4px;
  animation: fadeInUp 0.6s var(--sdn-ease-smooth) 0.1s both;
}
.card-wrapper :deep(h3) {
  font-size: 0.95rem; font-weight: 600;
  color: var(--sdn-accent); margin-top: 14px; margin-bottom: 6px;
}
.card-wrapper :deep(p) { font-size: 0.85rem; line-height: 1.65; }
.card-wrapper :deep(ul) { margin-top: 6px; }
.card-wrapper :deep(li) { font-size: 0.83rem; margin-bottom: 5px; }
</style>
