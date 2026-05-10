<!--
  FILE: layouts/chapter.vue
  PATH: /home/nora/Projects/slides/SDN-IDS/layouts/chapter.vue
  DESC: Chapter divider — warm white theme
-->
<template>
  <div class="slidev-layout chapter-layout">
    <div class="chapter-bg">
      <div class="chapter-gradient"></div>
      <div class="chapter-lines">
        <div v-for="i in 5" :key="i" class="line" :style="{ '--i': i }"></div>
      </div>
    </div>
    <div class="chapter-inner">
      <div class="chapter-number-display" v-if="$attrs.chapterNumber">
        {{ String($attrs.chapterNumber).padStart(2, '0') }}
      </div>
      <div class="chapter-content">
        <slot />
      </div>
      <div class="chapter-accent-bar"></div>
    </div>
  </div>
</template>
<style scoped>
.chapter-layout {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding: 60px 80px;
  overflow: hidden;
  background: var(--sdn-bg-deep);
}
.chapter-bg { position: absolute; inset: 0; }
.chapter-gradient {
  position: absolute; inset: 0;
  background:
    radial-gradient(ellipse 80% 60% at 0% 50%, rgba(79,70,229,0.05) 0%, transparent 60%),
    radial-gradient(ellipse 50% 80% at 100% 100%, rgba(8,145,178,0.04) 0%, transparent 60%);
}
.chapter-lines { position: absolute; inset: 0; }
.line {
  position: absolute; right: 0; height: 1px;
  background: linear-gradient(90deg, transparent, rgba(79,70,229,0.1), transparent);
  animation: fadeInRight 1s var(--sdn-ease-smooth) calc(var(--i) * 0.1s) both;
}
.line:nth-child(1) { top: 15%; width: 60%; }
.line:nth-child(2) { top: 30%; width: 45%; }
.line:nth-child(3) { top: 55%; width: 70%; }
.line:nth-child(4) { top: 70%; width: 40%; }
.line:nth-child(5) { top: 85%; width: 55%; }
.chapter-inner { position: relative; z-index: 1; width: 100%; }
.chapter-number-display {
  font-size: 8rem; font-weight: 900; line-height: 1;
  background: linear-gradient(135deg, rgba(79,70,229,0.1), rgba(8,145,178,0.06));
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
  position: absolute; top: -40px; right: 0;
  animation: fadeInRight 0.8s var(--sdn-ease-smooth) both;
  user-select: none;
}
.chapter-content { max-width: 600px; }
.chapter-content :deep(h1) {
  font-size: 2.6rem; font-weight: 800; line-height: 1.15;
  color: var(--sdn-text-primary); margin-bottom: 16px;
  animation: fadeInUp 0.7s var(--sdn-ease-smooth) 0.2s both;
}
.chapter-content :deep(h2) {
  font-size: 1.15rem; font-weight: 400;
  color: var(--sdn-text-secondary); line-height: 1.6;
  animation: fadeInUp 0.7s var(--sdn-ease-smooth) 0.35s both;
}
.chapter-content :deep(p) {
  font-size: 0.95rem; color: var(--sdn-text-muted);
  animation: fadeInUp 0.7s var(--sdn-ease-smooth) 0.5s both;
}
.chapter-accent-bar {
  width: 80px; height: 4px; border-radius: 2px;
  background: var(--sdn-gradient-primary);
  margin-top: 24px;
  animation: fadeInLeft 0.6s var(--sdn-ease-smooth) 0.6s both;
}
</style>
