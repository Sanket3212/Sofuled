<template>
  <div class="calendar-container">
    <div class="calendar-header">
      <button @click="prevYear" aria-label="Previous Year">«</button>
      <button @click="prevMonth" aria-label="Previous Month">‹</button>

      <div class="calendar-current-month">
        {{ monthNames[currentMonth] }} {{ currentYear }}
      </div>

      <button @click="nextMonth" aria-label="Next Month">›</button>
      <button @click="nextYear" aria-label="Next Year">»</button>
    </div>

    <div class="calendar-grid">
      <div v-for="day in days" :key="day" class="calendar-day-header">
        {{ day }}
      </div>

      <div
        v-for="(date, idx) in calendarDays"
        :key="idx"
        :class="[
          'calendar-date',
          { 'calendar-date--empty': !date },
          { 'calendar-date--today': isToday(date) },
          { 'calendar-date--selected': isSelected(date) }
        ]"
        @click="date && selectDate(date)"
      >
        {{ date || '' }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const days = ['S', 'M', 'T', 'W', 'T', 'F', 'S']
const monthNames = [
  'January', 'February', 'March', 'April', 'May', 'June',
  'July', 'August', 'September', 'October', 'November', 'December'
]

const today = new Date()
const currentYear = ref(today.getFullYear())
const currentMonth = ref(today.getMonth()) // 0-based month index
const selectedDate = ref(null)  // Will store a Date object or null

const calendarDays = computed(() => {
  const firstDayOfMonth = new Date(currentYear.value, currentMonth.value, 1).getDay()
  const daysInMonth = new Date(currentYear.value, currentMonth.value + 1, 0).getDate()

  const daysArray = []

  // fill empty days before first day
  for (let i = 0; i < firstDayOfMonth; i++) {
    daysArray.push(null)
  }

  // fill days of current month
  for (let day = 1; day <= daysInMonth; day++) {
    daysArray.push(day)
  }

  // fill empty days to complete weeks (optional)
  while (daysArray.length % 7 !== 0) {
    daysArray.push(null)
  }

  return daysArray
})

function prevMonth() {
  if (currentMonth.value === 0) {
    currentMonth.value = 11
    currentYear.value--
  } else {
    currentMonth.value--
  }
}

function nextMonth() {
  if (currentMonth.value === 11) {
    currentMonth.value = 0
    currentYear.value++
  } else {
    currentMonth.value++
  }
}

function prevYear() {
  currentYear.value--
}

function nextYear() {
  currentYear.value++
}

function isToday(date) {
  if (!date) return false
  return (
    date === today.getDate() &&
    currentMonth.value === today.getMonth() &&
    currentYear.value === today.getFullYear()
  )
}

function isSelected(date) {
  if (!date || !selectedDate.value) return false
  return (
    date === selectedDate.value.getDate() &&
    currentMonth.value === selectedDate.value.getMonth() &&
    currentYear.value === selectedDate.value.getFullYear()
  )
}

function selectDate(date) {
  if (!date) return
  selectedDate.value = new Date(currentYear.value, currentMonth.value, date)
}
</script>

<style scoped>
.calendar-container {
  max-width: 320px;
  margin: 20px auto;
  font-family: 'Inter', sans-serif;
  user-select: none;
  background: #fff;
  padding: 16px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgb(0 0 0 / 0.05);
}

.calendar-header {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  margin-bottom: 12px;
}

.calendar-header button {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  padding: 6px 10px;
  border-radius: 6px;
  color: #374151;
  transition: background-color 0.2s;
}

.calendar-header button:hover {
  background-color: #e5e7eb;
}

.calendar-current-month {
  font-weight: 600;
  font-size: 16px;
  min-width: 120px;
  text-align: center;
  color: #1f2937;
}

.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 6px;
}

.calendar-day-header {
  font-weight: 600;
  color: #6b7280;
  text-align: center;
  font-size: 14px;
}

.calendar-date {
  padding: 8px 0;
  text-align: center;
  border-radius: 9999px;
  cursor: pointer;
  color: #374151;
  font-size: 14px;
  transition: background-color 0.2s, color 0.2s;
}

.calendar-date--empty {
  cursor: default;
  background: none;
}

.calendar-date:hover:not(.calendar-date--empty) {
  background-color: #e5e7eb;
}

.calendar-date--today {
  border: 2px solid #10b981;
  font-weight: 600;
  color: #10b981;
}

.calendar-date--selected {
  background-color: #c5ff55;
  font-weight: 700;
  color: #000000;
}
</style>
