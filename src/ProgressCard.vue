<template>
  <div class="progress-card">
    <div class="card-header">
      <div class="icon-wrapper" :class="iconClass">
        {{ icon }}
      </div>
      <div>
        <div class="card-title">{{ title }}</div>
        <div class="card-subtitle">{{ subtitle }}</div>
      </div>
    </div>

    <div class="progress-section">
      <div class="progress-header">
        <span class="progress-label">Progress</span>
        <span class="progress-percent">{{ progress }}%</span>
      </div>
      <div class="progress-bar-bg">
        <div
          class="progress-bar-fill"
          :class="fillClass"
          :style="{ width: progress + '%' }"
        />
      </div>
    </div>

    <ul class="task-list">
      <li
        v-for="(task, index) in tasks"
        :key="index"
        class="task-item"
        :class="{ 'is-done': task.done }"
      >
        <BaseCheckbox :checked="task.done" :variant="checkboxVariant" />
        <span class="task-text" :class="{ 'is-completed': task.done }">
          {{ task.label }}
        </span>
      </li>
    </ul>

    <div class="stats-row">
      <div class="stat-box">
        <div class="stat-number">{{ doneCount }}</div>
        <div class="stat-label">Selesai</div>
      </div>
      <div class="stat-box">
        <div class="stat-number">{{ remainingCount }}</div>
        <div class="stat-label">Tersisa</div>
      </div>
      <div class="stat-box">
        <div class="stat-number">{{ tasks.length }}</div>
        <div class="stat-label">Total</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import BaseCheckbox from './BaseCheckbox.vue'

const props = defineProps({
  title: {
    type: String,
    required: true
  },
  subtitle: {
    type: String,
    default: ''
  },
  icon: {
    type: String,
    default: '📦'
  },
  variant: {
    type: String,
    default: 'primary',
    validator: (v) => ['primary', 'success'].includes(v)
  },
  tasks: {
    type: Array,
    required: true
  }
})

const iconClass = computed(() => ({
  'icon-web': props.variant === 'primary',
  'icon-app': props.variant === 'success'
}))

const fillClass = computed(() => ({
  'fill-web': props.variant === 'primary',
  'fill-app': props.variant === 'success'
}))

const checkboxVariant = computed(() =>
  props.variant === 'primary' ? 'primary' : 'success'
)

const doneCount = computed(() => props.tasks.filter(t => t.done).length)
const remainingCount = computed(() => props.tasks.length - doneCount.value)
const progress = computed(() => {
  if (props.tasks.length === 0) return 0
  return Math.round((doneCount.value / props.tasks.length) * 100)
})
</script>

<style scoped>
.progress-card {
  background: white;
  border-radius: 20px;
  padding: 28px;
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05), 0 10px 15px -3px rgba(0,0,0,0.05);
  border: 1px solid #e2e8f0;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.progress-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 25px -5px rgba(0,0,0,0.08), 0 10px 10px -5px rgba(0,0,0,0.04);
}

.card-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 20px;
}

.icon-wrapper {
  width: 48px;
  height: 48px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  flex-shrink: 0;
}

.icon-web { background: #dbeafe; color: #2563eb; }
.icon-app { background: #dcfce7; color: #16a34a; }

.card-title { font-size: 1.15rem; font-weight: 700; color: #1e293b; }
.card-subtitle { font-size: 0.8rem; color: #94a3b8; margin-top: 2px; }

.progress-section { margin-bottom: 24px; }
.progress-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}
.progress-label { font-size: 0.85rem; font-weight: 600; color: #475569; }
.progress-percent { font-size: 0.9rem; font-weight: 700; color: #1e293b; }

.progress-bar-bg {
  width: 100%;
  height: 10px;
  background: #f1f5f9;
  border-radius: 999px;
  overflow: hidden;
}

.progress-bar-fill {
  height: 100%;
  border-radius: 999px;
  transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.progress-bar-fill::after {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
  animation: shimmer 2s infinite;
}

.fill-web { background: linear-gradient(90deg, #3b82f6, #60a5fa); }
.fill-app { background: linear-gradient(90deg, #22c55e, #4ade80); }

.task-list { list-style: none; }
.task-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  padding: 10px 0;
  border-bottom: 1px solid #f1f5f9;
  transition: all 0.2s ease;
}
.task-item:last-child { border-bottom: none; }
.task-item:hover {
  background: #f8fafc;
  margin: 0 -12px;
  padding-left: 12px;
  padding-right: 12px;
  border-radius: 10px;
}

.task-text {
  font-size: 0.9rem;
  color: #334155;
  transition: all 0.25s ease;
  line-height: 1.5;
}
.task-text.is-completed {
  color: #94a3b8;
  text-decoration: line-through;
}

.stats-row {
  display: flex;
  gap: 16px;
  margin-top: 20px;
  padding-top: 20px;
  border-top: 1px solid #f1f5f9;
}
.stat-box {
  flex: 1;
  text-align: center;
  padding: 12px;
  background: #f8fafc;
  border-radius: 12px;
  transition: transform 0.2s ease;
}
.stat-box:hover { transform: scale(1.05); }
.stat-number { font-size: 1.3rem; font-weight: 800; color: #1e293b; }
.stat-label {
  font-size: 0.7rem;
  color: #94a3b8;
  margin-top: 2px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

@keyframes shimmer {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}
</style>
