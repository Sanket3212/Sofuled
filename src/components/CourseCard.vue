<template>
  <div class="card-container">
    <div v-for="(card, index) in cards" :key="index" class="content-card" :style="{ backgroundColor: card.bgColor }">
      <div class="card-top">
        <div class="icon-circle" :style="{ backgroundColor: card.iconBg }">
          <span class="icon">{{ card.icon }}</span>
        </div>
        <div class="text-group">
          <h3 class="title">{{ card.title }}</h3>
          <p class="subtitle">{{ card.lessons }} Lessons</p>
        </div>
      </div>

      <div class="card-bottom">
        <div class="info-block">
          <p class="label">Rate</p>
          <p class="value"><span class="star">⭐</span> {{ card.rating }}</p>
        </div>
        <div class="info-block">
          <p class="label">Type</p>
          <p class="value">{{ card.type }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const cards = [
  {
    icon: '📝',
    title: 'Content Writing',
    lessons: 12,
    rating: 4.8,
    type: 'Data Research',
    iconBg: '#fef3e7',
    bgColor: '#ffffff',
  },
  {
    icon: '💬',
    title: 'Usability Testing',
    lessons: 15,
    rating: 5.0,
    type: 'UI/UX Design',
    iconBg: '#f0fdf4',
    bgColor: '#ffffff',
  },
  {
    icon: '📷',
    title: 'Photography',
    lessons: 8,
    rating: 4.6,
    type: 'Art and Design',
    iconBg: '#f3f4ff',
    bgColor: '#ffffff',
  },
];
</script>

<style scoped>
.card-container {
  display: flex;
  gap: 16px;
  flex-wrap: wrap;
}

.content-card {
  border-radius: 16px;
  padding: 16px;
  width: 240px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  font-family: 'Inter', sans-serif;
}

.card-top {
  display: flex;
  align-items: center;
  gap: 12px;
}

.icon-circle {
  border-radius: 50%;
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon {
  font-size: 20px;
}

.text-group .title {
  font-size: 16px;
  font-weight: 600;
  color: #1f2937;
  margin: 0;
}

.text-group .subtitle {
  font-size: 13px;
  color: #9ca3af;
  margin: 0;
}

.card-bottom {
  display: flex;
  justify-content: space-between;
  margin-top: 16px;
}

.info-block {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.label {
  font-size: 12px;
  color: #9ca3af;
}

.value {
  font-size: 14px;
  font-weight: 600;
  color: #1f2937;
}

.star {
  color: #fbbf24;
  margin-right: 2px;
}
</style>
