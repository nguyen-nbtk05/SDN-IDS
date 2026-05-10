<!--
  FILE: layouts/cover-custom.vue
  PATH: /home/nora/Projects/slides/SDN-IDS/layouts/cover-custom.vue
  DESC: Custom cover layout with animated network background
-->
<template>
  <div class="slidev-layout cover-custom">
    <!-- Animated background mesh -->
    <div class="cover-bg">
      <div class="mesh-gradient"></div>
      <div class="grid-pattern"></div>
      <div class="floating-orbs">
        <div class="orb orb-1"></div>
        <div class="orb orb-2"></div>
        <div class="orb orb-3"></div>
      </div>
    </div>

    <!-- Content -->
    <div class="cover-content">
      <slot />
    </div>

    <!-- Bottom accent line -->
    <div class="cover-bottom-line"></div>
  </div>
</template>

<style scoped>
.cover-custom {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 40px 60px;
  overflow: hidden;
  background: var(--sdn-bg-deep);
}

.cover-bg {
  position: absolute;
  inset: 0;
  z-index: 0;
}

.mesh-gradient {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse 80% 60% at 20% 40%, rgba(99, 102, 241, 0.15) 0%, transparent 70%),
    radial-gradient(ellipse 60% 50% at 80% 60%, rgba(6, 182, 212, 0.12) 0%, transparent 70%),
    radial-gradient(ellipse 50% 40% at 50% 80%, rgba(16, 185, 129, 0.08) 0%, transparent 70%);
}

.grid-pattern {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(148, 163, 184, 0.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(148, 163, 184, 0.04) 1px, transparent 1px);
  background-size: 40px 40px;
}

.floating-orbs {
  position: absolute;
  inset: 0;
}

.orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(60px);
  animation: float 6s ease-in-out infinite;
}

.orb-1 {
  width: 300px;
  height: 300px;
  top: -50px;
  right: -50px;
  background: rgba(99, 102, 241, 0.12);
  animation-delay: 0s;
}

.orb-2 {
  width: 200px;
  height: 200px;
  bottom: -30px;
  left: 10%;
  background: rgba(6, 182, 212, 0.1);
  animation-delay: -2s;
}

.orb-3 {
  width: 150px;
  height: 150px;
  top: 40%;
  right: 20%;
  background: rgba(16, 185, 129, 0.08);
  animation-delay: -4s;
}

.cover-content {
  position: relative;
  z-index: 1;
  width: 100%;
  max-width: 860px;
  text-align: center;
}

.cover-content :deep(h1) {
  font-size: 2.4rem;
  font-weight: 900;
  line-height: 1.15;
  margin-bottom: 16px;
  background: linear-gradient(135deg, #f1f5f9 0%, #cbd5e1 50%, #94a3b8 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: fadeInUp 0.8s var(--sdn-ease-smooth) both;
}

.cover-content :deep(h2) {
  font-size: 1.2rem;
  font-weight: 500;
  color: var(--sdn-text-secondary);
  margin-bottom: 12px;
  animation: fadeInUp 0.8s var(--sdn-ease-smooth) 0.15s both;
}

.cover-content :deep(h3) {
  font-size: 1rem;
  font-weight: 400;
  color: var(--sdn-text-muted);
  animation: fadeInUp 0.8s var(--sdn-ease-smooth) 0.3s both;
}

.cover-content :deep(p) {
  font-size: 0.9rem;
  color: var(--sdn-text-muted);
  animation: fadeInUp 0.8s var(--sdn-ease-smooth) 0.45s both;
}

.cover-bottom-line {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: var(--sdn-gradient-primary);
}
</style>
