<template>
  <div class="math-formula" v-html="formulaHtml"></div>
</template>

<script setup>
import { computed } from 'vue'
import katex from 'katex'
import 'katex/dist/katex.min.css'

const props = defineProps({
  expression: {
    type: String,
    required: true,
  },
})

const formulaHtml = computed(() =>
  katex.renderToString(props.expression, {
    displayMode: true,
    throwOnError: false,
  }),
)
</script>

<style scoped>
.math-formula {
  margin: 16px 0;
  border: 1px solid var(--sdn-border);
  border-radius: 14px;
  background: var(--sdn-surface-soft);
  color: var(--sdn-text-primary);
  font-size: 1.16rem;
  overflow-x: auto;
  padding: 16px;
}

.math-formula :deep(.katex-display) {
  margin: 0 !important;
}
</style>
