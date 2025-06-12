<template>
  <div class="schedule-card">
    <h3 class="card-title">Daily Schedule</h3>
    <div
      v-for="(item, i) in schedule"
      :key="i"
      class="schedule-item"
      :class="{ 'no-border': i === schedule.length - 1 }"
    >
      <div class="left-section">
        <div class="icon-circle" :style="{ backgroundColor: item.bg }">
          <span class="emoji">{{ item.icon }}</span>
        </div>
        <div class="schedule-info">
          <p class="schedule-title">{{ item.title }}</p>
          <p class="schedule-type">{{ item.type }}</p>
        </div>
      </div>
      <div class="schedule-icon">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="arrow-icon"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
        </svg>
      </div>
    </div>
  </div>
</template>

<script setup>
const schedule = [
  { title: 'Design System', type: 'Lecture - Class', icon: '🧩', bg: '#fde7de' },
  { title: 'Typography', type: 'Group - Test', icon: '💡', bg: '#e4e8fb' },
  { title: 'Color Style', type: 'Group - Test', icon: '🎨', bg: '#edf7d6' },
  { title: 'Visual Design', type: 'Lecture - Test', icon: '🔄', bg: '#fde7de' }
]
</script>

<style scoped>
.schedule-card {
  background-color: white;
  padding: 16px;
  border-radius: 16px;
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.05);
  font-family: 'Inter', sans-serif;
  width: 280px;
}

.card-title {
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 16px;
  color: #111827;
}

.schedule-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px solid #e5e7eb;
}

.schedule-item.no-border {
  border-bottom: none;
}

.left-section {
  display: flex;
  align-items: center;
  gap: 12px;
}

.icon-circle {
  width: 44px;
  height: 44px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.emoji {
  font-size: 20px;
}

.schedule-info {
  display: flex;
  flex-direction: column;
}

.schedule-title {
  font-size: 14px;
  font-weight: 600;
  margin: 0;
  color: #111827;
}

.schedule-type {
  font-size: 12px;
  color: #9ca3af;
  margin-top: 4px;
}

.schedule-icon {
  display: flex;
  align-items: center;
}

.arrow-icon {
  width: 18px;
  height: 18px;
  color: #9ca3af;
}
</style>
