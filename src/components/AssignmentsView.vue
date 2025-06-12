<template>
  <div class="assignments-container">
    <div class="assignments-header">
      <h3 class="assignments-title">Assignments</h3>
      <button class="assignments-add">+</button>
    </div>

    <div
      v-for="(a, i) in assignments"
      :key="i"
      class="assignment-item"
    >
      <div class="left-section">
        <div class="icon-badge" :style="{ backgroundColor: a.bg }">
          <span class="emoji">{{ a.icon }}</span>
        </div>
        <div class="assignment-info">
          <p class="assignment-title">{{ a.title }}</p>
          <p class="assignment-date">{{ a.date }}</p>
        </div>
      </div>
      <span :class="getStatusClass(a.status)">{{ a.status }}</span>
    </div>
  </div>
</template>

<script setup>
const assignments = [
  {
    title: 'Methods of data',
    date: '02 July, 10:30 AM',
    status: 'In progress',
    icon: '🧠',
    bg: '#fdf2f8'
  },
  {
    title: 'Market Research',
    date: '14 June, 12:45 AM',
    status: 'Completed',
    icon: '📊',
    bg: '#f0fdf4'
  },
  {
    title: 'Data Collection',
    date: '12 May, 11:00 AM',
    status: 'Upcoming',
    icon: '📁',
    bg: '#fff7ed'
  }
]

const getStatusClass = (status) => {
  switch (status) {
    case 'In progress':
      return 'status-badge in-progress'
    case 'Completed':
      return 'status-badge completed'
    case 'Upcoming':
      return 'status-badge upcoming'
    default:
      return 'status-badge default'
  }
}
</script>

<style scoped>
.assignments-container {
  background-color: #ffffff;
  padding: 12px;
  border-radius: 12px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.04);
  width: 260px;
  font-family: 'Inter', sans-serif;
}

.assignments-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 14px;
}

.assignments-title {
  font-size: 14px;
  font-weight: 600;
  color: #1f2937;
}

.assignments-add {
  font-size: 16px;
  font-weight: bold;
  color: #7ed957;
  background-color: #f3fef5;
  border: none;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.assignment-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 0;
  border-bottom: 1px solid #f3f4f6;
}

.assignment-item:last-child {
  border-bottom: none;
}

.left-section {
  display: flex;
  align-items: center;
  gap: 8px;
}

.icon-badge {
  width: 34px;
  height: 34px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.emoji {
  font-size: 16px;
}

.assignment-info {
  display: flex;
  flex-direction: column;
}

.assignment-title {
  font-size: 12px;
  font-weight: 600;
  color: #111827;
  margin: 0;
}

.assignment-date {
  font-size: 10px;
  color: #9ca3af;
  margin-top: 2px;
}

.status-badge {
  font-size: 10px;
  padding: 4px 8px;
  border-radius: 9999px;
  font-weight: 500;
  white-space: nowrap;
}

.status-badge.in-progress {
  background-color: #e0f2fe;
  color: #0284c7;
}

.status-badge.completed {
  background-color: #dcfce7;
  color: #16a34a;
}

.status-badge.upcoming {
  background-color: #fef9c3;
  color: #ca8a04;
}

.status-badge.default {
  background-color: #e5e7eb;
  color: #6b7280;
}

</style>
