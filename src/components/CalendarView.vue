<template>
  <div class="calendar-grid">
    <div v-for="day in days" :key="day" class="calendar-day-header">
      {{ day }}
    </div>

    <div
      v-for="(date, index) in dates"
      :key="index"
      class="calendar-date"
      :class="{ 'calendar-date-active': date === 17 }"
    >
      {{ date }}
    </div>
  </div>
</template>

<script setup>
const days = ['S', 'M', 'T', 'W', 'T', 'F', 'S']
const dates = [
  28, 29, 30, 31, 1, 2, 3,
  4, 5, 6, 7, 8, 9, 10,
  11, 12, 13, 14, 15, 16, 17,
  18, 19, 20, 21, 22, 23, 24,
  25, 26, 27, 28, 29, 30, 31, 1
]
</script>

<style scoped>
.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 4px;
  font-size: 14px;
  text-align: center;
}

.calendar-day-header {
  font-weight: 600;
  color: #6b7280; /* gray-500 */
}

.calendar-date {
  padding: 6px 0;
  border-radius: 9999px;
  color: #374151; /* gray-700 */
  cursor: pointer;
  transition: background 0.2s;
}

.calendar-date:hover {
  background-color: #e5e7eb; /* gray-200 */
}

.calendar-date-active {
  background-color: #c5ff55;
  color: #000000;
  font-weight: 600;
}
</style>
