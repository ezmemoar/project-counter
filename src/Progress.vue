<template>
  <div class="progress-page">
    <div class="container">
      <header class="page-header">
        <h1>📦 Progress Project Sukabumi Logistik</h1>
      </header>

      <div class="overall-progress" :class="{ 'celebration': justCompleted }">
        <div class="overall-title">📊 Progress Keseluruhan Project</div>
        <div class="overall-bar-bg">
          <div class="overall-bar-fill" :style="{ width: overallProgress + '%' }" />
        </div>
        <div class="overall-percent">{{ overallProgress }}%</div>
      </div>

      <div class="projects-grid">
        <ProgressCard
          title="Website Profil Perusahaan"
          icon="🌐"
          variant="primary"
          :tasks="websiteTasks"
        />

        <ProgressCard
          title="Aplikasi Sukabumi Logistik"
          icon="📱"
          variant="success"
          :tasks="appTasks"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import ProgressCard from './ProgressCard.vue'

// ===================== BUSINESS LOGIC / DATA =====================
// Data task Website — status dikontrol dari sini
const websiteTasks = ref([
  { label: 'Halaman Beranda', done: true },
  { label: 'Halaman Tentang Kami', done: true },
  { label: 'Halaman Blog', done: true },
  { label: 'Halaman Detail Blog', done: true },
  { label: 'Halaman Track Pengiriman', done: true },
  { label: 'Integrasi Track Pengiriman', done: false },
  { label: 'Optimisasi SEO', done: true },
  { label: 'Penyesuaian konten', done: true },
  { label: 'Penyesuaian layout website', done: true },
  { label: 'Deployment', done: true },
])

// Data task Aplikasi — status dikontrol dari sini
const appTasks = ref([
  { label: 'Halaman Login', done: false },
  { label: 'Halaman Register', done: false },
  { label: 'Halaman Lupa Password', done: false },
  { label: 'Halaman Beranda', done: false },
  { label: 'Halaman Akun', done: false },
  { label: 'Halaman List History Pengiriman', done: false },
  { label: 'Halaman Detail History', done: false },
  { label: 'Fitur Cetak Resi', done: false },
  { label: 'Menu Kirim Paket', done: false },
  { label: 'Menu Check Progress Paket Mitra', done: false },
  { label: 'Menu Pesan', done: false },
  { label: 'Integrasi Raja Ongkir', done: false },
  { label: 'Deployment', done: false },
])

// ===================== COMPUTED =====================
const websiteDone = computed(() => websiteTasks.value.filter(t => t.done).length)
const appDone = computed(() => appTasks.value.filter(t => t.done).length)

const overallProgress = computed(() => {
  const total = websiteTasks.value.length + appTasks.value.length
  const done = websiteDone.value + appDone.value
  if (total === 0) return 0
  return Math.round((done / total) * 100)
})

// ===================== EFFECTS =====================
const justCompleted = ref(false)
let prevOverall = 0

watch(overallProgress, (newVal, oldVal) => {
  prevOverall = oldVal || 0
  if (newVal === 100 && prevOverall < 100) {
    justCompleted.value = true
    spawnConfetti()
    setTimeout(() => { justCompleted.value = false }, 600)
  }
})

function spawnConfetti() {
  const colors = ['#3b82f6', '#22c55e', '#8b5cf6', '#f59e0b', '#ef4444', '#ec4899']
  for (let i = 0; i < 40; i++) {
    const el = document.createElement('div')
    el.style.cssText = `
      position: fixed;
      left: ${Math.random() * 100}vw;
      top: -10px;
      width: ${6 + Math.random() * 6}px;
      height: ${6 + Math.random() * 6}px;
      background: ${colors[Math.floor(Math.random() * colors.length)]};
      border-radius: ${Math.random() > 0.5 ? '50%' : '2px'};
      pointer-events: none;
      z-index: 9999;
      animation: confettiFall ${1 + Math.random() * 1.5}s ease-out forwards;
      animation-delay: ${Math.random() * 0.3}s;
    `
    document.body.appendChild(el)
    setTimeout(() => el.remove(), 2500)
  }
}
</script>

<style scoped>
.progress-page {
  font-family: 'Inter', sans-serif;
  background: #f0f4f8;
  min-height: 100vh;
  padding: 70px 24px;
}

.container {
  max-width: 900px;
  margin: 0 auto;
}

.page-header {
  text-align: center;
  margin-bottom: 40px;
  animation: fadeInDown 0.8s ease-out;
}

.page-header h1 {
  font-size: 2rem;
  font-weight: 800;
  color: #1e293b;
  margin-bottom: 8px;
}

.page-header p {
  color: #64748b;
  font-size: 1rem;
}

.projects-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24px;
  animation: fadeInUp 0.8s ease-out 0.1s both;
}

@media (min-width: 768px) {
  .projects-grid {
    grid-template-columns: 1fr 1fr;
  }
}

.overall-progress {
  background: white;
  border-radius: 20px;
  padding: 28px;
  margin-bottom: 24px;
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05);
  border: 1px solid #e2e8f0;
  text-align: center;
  animation: fadeInUp 0.8s ease-out 0.3s both;
}

.overall-title {
  font-size: 1rem;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 16px;
}

.overall-bar-bg {
  width: 100%;
  height: 16px;
  background: #f1f5f9;
  border-radius: 999px;
  overflow: hidden;
  margin-bottom: 12px;
}

.overall-bar-fill {
  height: 100%;
  border-radius: 999px;
  background: linear-gradient(90deg, #8b5cf6, #a78bfa, #c4b5fd);
  transition: width 0.8s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
}

.overall-bar-fill::after {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
  animation: shimmer 2s infinite;
}

.overall-percent {
  font-size: 2rem;
  font-weight: 800;
  color: #7c3aed;
}

.overall-label {
  font-size: 0.85rem;
  color: #94a3b8;
}

.celebration {
  animation: pulse 0.5s ease-in-out;
}

@keyframes fadeInDown {
  from { opacity: 0; transform: translateY(-20px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes shimmer {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.02); }
}

@keyframes confettiFall {
  0% { transform: translateY(0) rotate(0deg); opacity: 1; }
  100% { transform: translateY(300px) rotate(720deg); opacity: 0; }
}
</style>
