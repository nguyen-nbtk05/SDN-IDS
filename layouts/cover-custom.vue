<template>
  <div class="slidev-layout cover-custom">
    <slot name="particles" />
    <div class="cover-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.cover-custom {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100%;
  background: linear-gradient(135deg, #0F2940 0%, #1B3A5C 40%, #2A5580 100%);
  overflow: hidden;
}
.cover-custom::before {
  display: none;
}
.cover-content {
  position: relative;
  z-index: 1;
  text-align: center;
  padding: 2rem;
}
</style>
