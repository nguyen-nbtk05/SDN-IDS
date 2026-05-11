<!--
  FILE: components/CoverSlide.vue
  PATH: /home/nora/Projects/slides/SDN-IDS/components/CoverSlide.vue
  DESC: Cover slide — warm white theme, full-height
-->
<template>
  <div class="cover-slide">
    <div class="cover-bg-mesh"></div>
    <div class="cover-decor-circle cover-decor-1"></div>
    <div class="cover-decor-circle cover-decor-2"></div>
    <div class="cover-decor-circle cover-decor-3"></div>

    <div class="cover-inner">
      <div class="cover-badge animate-fade-in-down">
        <span class="badge">{{ badge }}</span>
      </div>

      <h1 class="cover-title animate-fade-in-up">
        <slot name="title">
          PHÁT HIỆN VÀ NGĂN CHẶN
          <span class="gradient-text">TẤN CÔNG MẠNG</span> DỰA TRÊN
          MÔ HÌNH <span class="gradient-text-accent">SDN</span>
        </slot>
      </h1>

      <p class="cover-subtitle animate-fade-in-up delay-200">
        <slot name="subtitle">
          SỬ DỤNG THỐNG KÊ LUỒNG VÀ PHÂN TÍCH ENTROPY
        </slot>
      </p>

      <div class="cover-divider animate-fade-in-left delay-300"></div>

      <div class="cover-meta animate-fade-in-up delay-400">
        <div class="meta-grid">
          <template v-if="advisor">
            <div class="meta-label">Giảng viên:</div>
            <div class="meta-value">{{ advisor }}</div>
          </template>
          <template v-if="student">
            <div class="meta-label">Sinh viên thực hiện:</div>
            <div class="meta-value">
              <template v-if="Array.isArray(student)">
                <div v-for="(s, i) in student" :key="i">{{ s }}</div>
              </template>
              <template v-else>
                {{ student }}
              </template>
            </div>
          </template>
        </div>
      </div>
    </div>

    <div class="cover-bottom-accent"></div>
  </div>
</template>

<script setup>
defineProps({
  badge: { type: String, default: 'CHUYÊN ĐỀ MẠNG MÁY TÍNH 1' },
  advisor: { type: String, default: 'Trần Vĩnh Phúc' },
  student: { type: [String, Array], default: () => ['Nguyễn Bá Thiều Khôi Nguyên — MSSV: 2312702', 'Hồ Quốc Long — MSSV: 2312675'] }
})
</script>

<style scoped>
.cover-slide {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--sdn-bg-deep);
  overflow: hidden;
}

.cover-bg-mesh {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse 70% 50% at 15% 35%, rgba(79,70,229,0.07) 0%, transparent 70%),
    radial-gradient(ellipse 50% 40% at 85% 65%, rgba(8,145,178,0.06) 0%, transparent 70%);
  z-index: 0;
}

.cover-decor-circle {
  position: absolute;
  border-radius: 50%;
  border: 1px solid rgba(79,70,229,0.08);
  z-index: 0;
}
.cover-decor-1 { width: 400px; height: 400px; top: -120px; right: -80px; }
.cover-decor-2 { width: 250px; height: 250px; bottom: -60px; left: -40px; border-color: rgba(8,145,178,0.06); }
.cover-decor-3 { width: 180px; height: 180px; top: 50%; right: 15%; border-color: rgba(5,150,105,0.06); }

.cover-inner {
  position: relative;
  z-index: 2;
  text-align: center;
  max-width: 780px;
}

.cover-badge { margin-bottom: 18px; }

.cover-title {
  font-size: 2.2rem !important;
  font-weight: 900 !important;
  line-height: 1.2 !important;
  color: var(--sdn-text-primary);
  margin-bottom: 16px;
}

.cover-subtitle {
  font-size: 1rem;
  color: var(--sdn-text-secondary);
  font-weight: 400;
  margin-bottom: 24px;
  text-align: center;
  margin-left: auto;
  margin-right: auto;
}

.cover-divider {
  width: 200px; height: 3px; border-radius: 2px;
  background: var(--sdn-gradient-primary);
  margin: 0 auto 24px;
}

.cover-meta {
  display: flex;
  justify-content: center;
  width: 100%;
}

.meta-grid {
  display: inline-grid;
  grid-template-columns: max-content max-content;
  column-gap: 16px;
  row-gap: 8px;
  text-align: left;
  font-size: 0.85rem;
}

.meta-label {
  color: var(--sdn-text-muted);
  font-weight: 500;
  text-align: left;
}

.meta-value {
  color: var(--sdn-text-primary);
  font-weight: 600;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.cover-bottom-accent {
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 3px;
  background: var(--sdn-gradient-primary);
  z-index: 5;
}
</style>
