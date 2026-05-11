<!--
  FILE: global-bottom.vue
  PATH: /home/nora/Projects/slides/SDN-IDS/global-bottom.vue
  DESC: Global footer — warm white theme
-->
<script setup>
import { useNav } from '@slidev/client'
const nav = useNav()
</script>

<template>
  <footer v-if="nav.currentLayout !== 'cover-custom' && nav.currentLayout !== 'end'" class="global-footer">
    <div class="footer-left">
      <span class="footer-brand">SDN-IDS</span>
      <span class="footer-sep">·</span>
      <span class="footer-title">HỆ THỐNG PHÁT HIỆN VÀ NGĂN CHẶN TẤN CÔNG MẠNG</span>
    </div>
    <div class="footer-right">
      <span class="footer-page">{{ nav.currentPage }} / {{ nav.total }}</span>
    </div>
  </footer>
</template>
<style scoped>
.global-footer {
  position: absolute;
  bottom: 0; left: 0; right: 0;
  display: flex; justify-content: space-between; align-items: center;
  padding: 8px 32px;
  font-family: var(--sdn-font-sans);
  font-size: 0.65rem;
  color: var(--sdn-text-muted);
  border-top: 1px solid rgba(0,0,0,0.04);
  background: rgba(250,248,245,0.7);
  backdrop-filter: blur(8px);
  z-index: 10;
}
.footer-left { display: flex; align-items: center; gap: 6px; }
.footer-brand { font-weight: 700; color: var(--sdn-primary); letter-spacing: 0.05em; }
.footer-sep { opacity: 0.3; }
.footer-title { opacity: 0.8; }
.footer-page { font-weight: 600; font-variant-numeric: tabular-nums; }
</style>
