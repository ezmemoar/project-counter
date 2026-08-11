<template>
  <div class="base-checkbox" :class="variantClass">
    <span v-if="checked" class="check-icon">✓</span>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  checked: {
    type: Boolean,
    default: false
  },
  variant: {
    type: String,
    default: 'primary',
    validator: (v) => ['primary', 'success'].includes(v)
  }
})

const variantClass = computed(() => ({
  'is-checked': props.checked,
  [`variant-${props.variant}`]: true
}))
</script>

<style scoped>
.base-checkbox {
  width: 20px;
  height: 20px;
  min-width: 20px;
  border: 2px solid #cbd5e1;
  border-radius: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  margin-top: 2px;
}

.base-checkbox.is-checked.variant-primary {
  background: #3b82f6;
  border-color: #3b82f6;
}

.base-checkbox.is-checked.variant-success {
  background: #22c55e;
  border-color: #22c55e;
}

.check-icon {
  color: white;
  font-size: 12px;
  animation: popIn 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}

@keyframes popIn {
  from { transform: scale(0); }
  to { transform: scale(1); }
}
</style>
