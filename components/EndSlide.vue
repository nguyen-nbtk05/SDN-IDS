<!--
  FILE: components/EndSlide.vue
  PATH: /home/nora/Projects/slides/SDN-IDS/components/EndSlide.vue
  DESC: End/thank you slide — warm white, full-height
-->
<template>
  <div class="end-slide">
    <div class="end-bg-mesh"></div>
    <div class="end-decor end-decor-1"></div>
    <div class="end-decor end-decor-2"></div>

    <div class="end-inner">
      <div class="end-icon animate-scale-in">🎓</div>
      <h1 class="end-title animate-fade-in-up delay-200">
        Cảm ơn quý Thầy/Cô<br/>đã lắng nghe!
      </h1>
      <div class="end-divider animate-fade-in-left delay-400"></div>
      <p class="end-subtitle animate-fade-in-up delay-500">Phần hỏi đáp — Q&amp;A</p>
      <div class="end-contact animate-fade-in-up delay-600">
        <span>📧 {{ email }}</span>
        <span style="opacity:0.3;">•</span>
        <span>🔗 {{ github }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  email: { type: String, default: 'student@university.edu.vn' },
  github: { type: String, default: 'github.com/sdn-ids' }
})
</script>

<style scoped>
.end-slide {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--sdn-bg-deep);
  overflow: hidden;
}

.end-bg-mesh {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse 60% 50% at 50% 40%, rgba(79,70,229,0.05) 0%, transparent 70%);
}

.end-decor {
  position: absolute;
  border-radius: 50%;
  border: 1px solid rgba(79,70,229,0.06);
}
.end-decor-1 { width: 350px; height: 350px; bottom: -100px; right: -80px; }
.end-decor-2 { width: 200px; height: 200px; top: -60px; left: -40px; border-color: rgba(8,145,178,0.05); }

.end-inner {
  position: relative;
  z-index: 2;
  text-align: center;
}

.end-icon {
  font-size: 3.5rem;
  margin-bottom: 20px;
}

.end-title {
  font-size: 2rem !important;
  font-weight: 800 !important;
  color: var(--sdn-text-primary);
  line-height: 1.3 !important;
  margin-bottom: 18px;
}

.end-divider {
  width: 80px; height: 3px; border-radius: 2px;
  background: var(--sdn-gradient-primary);
  margin: 0 auto 18px;
}

.end-subtitle {
  font-size: 1.1rem;
  color: var(--sdn-primary);
  font-weight: 600;
  margin-bottom: 24px;
}

.end-contact {
  display: flex;
  gap: 12px;
  justify-content: center;
  font-size: 0.82rem;
  color: var(--sdn-text-muted);
}
</style>
