<template>
  <div class="course-progress-card">
    <div class="course-progress-header">
      <h3>Course You're Taking</h3>
      <div class="course-actions">
        <button class="dropdown" @click="toggleDropdown">
          {{ filter }} ▼
        </button>
        <button class="add-btn" @click="showForm = true">+</button>
      </div>
    </div>

    <!-- Dropdown Menu -->
    <div v-if="dropdownOpen" class="dropdown-menu">
      <div class="dropdown-item" @click="setFilter('Active')">Active</div>
      <div class="dropdown-item" @click="setFilter('Completed')">Completed</div>
    </div>

    <!-- Add Course Form -->
    <div v-if="showForm" class="add-course-form">
      <h4>Add New Course</h4>
      <form @submit.prevent="addCourse">
        <input v-model="newCourse.title" placeholder="Course Title" required />
        <input v-model="newCourse.author" placeholder="Author" required />
        <input v-model="newCourse.remaining" placeholder="Remaining Time (e.g. 5h 30m)" required />
        <input
          v-model.number="newCourse.progress"
          type="number"
          min="0"
          max="100"
          placeholder="Progress %"
          required
        />
        <div class="form-buttons">
          <button type="submit" class="submit-btn">Add</button>
          <button type="button" class="cancel-btn" @click="cancelForm">Cancel</button>
        </div>
      </form>
    </div>

    <!-- Courses List -->
    <div
      v-for="(course, i) in filteredCourses"
      :key="i"
      class="course-row"
    >
      <div class="course-left">
        <div class="icon-box" :style="{ backgroundColor: course.color }">
          <img :src="course.icon" alt="icon" />
        </div>
        <div>
          <p class="course-title">{{ course.title }}</p>
          <div class="author">
            <img :src="course.avatar" />
            <span>{{ course.author }}</span>
          </div>
        </div>
      </div>

      <div class="course-right">
        <div class="remaining-text">
          <span class="label">Remaining</span>
          <span class="time">{{ course.remaining }}</span>
        </div>
        <div class="progress-circle">
          <svg viewBox="0 0 36 36">
            <path
              class="bg"
              d="M18 2.0845
                a 15.9155 15.9155 0 0 1 0 31.831
                a 15.9155 15.9155 0 0 1 0 -31.831"
            />
            <path
              class="fg"
              :stroke-dasharray="course.progress + ', 100'"
              d="M18 2.0845
                a 15.9155 15.9155 0 0 1 0 31.831
                a 15.9155 15.9155 0 0 1 0 -31.831"
            />
          </svg>
          <span class="percent">{{ course.progress }}%</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref, computed } from 'vue'

const courses = reactive([
  {
    title: '3D Design Course',
    author: 'Micheal Andrew',
    remaining: '8h 45 min',
    progress: 45,
    avatar: 'https://i.pravatar.cc/30?img=1',
    icon: 'https://cdn-icons-png.flaticon.com/512/8771/8771366.png',
    color: '#e8d2ff',
  },
  {
    title: 'Development Basics',
    author: 'Natalia Varnan',
    remaining: '18h 12 min',
    progress: 100,
    avatar: 'https://i.pravatar.cc/30?img=2',
    icon: 'https://cdn-icons-png.flaticon.com/512/1159/1159633.png',
    color: '#ffdee5',
  },
])

const showForm = ref(false)
const dropdownOpen = ref(false)
const filter = ref('Active')

const newCourse = reactive({
  title: '',
  author: '',
  remaining: '',
  progress: 0,
})

function addCourse() {
  if (newCourse.progress < 0) newCourse.progress = 0
  if (newCourse.progress > 100) newCourse.progress = 100

  // Default avatar, icon, color
  courses.push({
    title: newCourse.title,
    author: newCourse.author,
    remaining: newCourse.remaining,
    progress: newCourse.progress,
    avatar: 'https://i.pravatar.cc/30?img=3',
    icon: 'https://cdn-icons-png.flaticon.com/512/1006/1006545.png',
    color: '#c7d8f5',
  })

  resetForm()
  showForm.value = false
}

function cancelForm() {
  resetForm()
  showForm.value = false
}

function resetForm() {
  newCourse.title = ''
  newCourse.author = ''
  newCourse.remaining = ''
  newCourse.progress = 0
}

function toggleDropdown() {
  dropdownOpen.value = !dropdownOpen.value
}

function setFilter(value) {
  filter.value = value
  dropdownOpen.value = false
}

const filteredCourses = computed(() => {
  if (filter.value === 'Active') {
    return courses.filter(course => course.progress < 100)
  } else if (filter.value === 'Completed') {
    return courses.filter(course => course.progress === 100)
  }
  return courses
})
</script>

<style scoped>
.course-progress-card {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 20px;
  font-family: 'Inter', sans-serif;
  position: relative;
}

.course-progress-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.course-progress-header h3 {
  font-size: 16px;
  font-weight: 600;
}

.course-actions {
  display: flex;
  align-items: center;
  gap: 10px;
  position: relative;
}

.dropdown {
  background: white;
  border: 1px solid #ccc;
  border-radius: 16px;
  padding: 4px 12px;
  font-size: 14px;
  cursor: pointer;
  user-select: none;
}

.add-btn {
  background: #d3f365;
  border: none;
  font-size: 20px;
  border-radius: 50%;
  width: 28px;
  height: 28px;
  cursor: pointer;
}

/* Dropdown menu */
.dropdown-menu {
  position: absolute;
  top: 42px;
  right: 60px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 8px rgb(0 0 0 / 0.1);
  width: 120px;
  z-index: 10;
}

.dropdown-item {
  padding: 10px 16px;
  cursor: pointer;
  font-size: 14px;
  border-bottom: 1px solid #eee;
}

.dropdown-item:last-child {
  border-bottom: none;
}

.dropdown-item:hover {
  background-color: #f0f0f0;
}

/* Add Course Form Styles */
.add-course-form {
  background: white;
  border-radius: 14px;
  padding: 16px;
  margin-bottom: 16px;
}

.add-course-form h4 {
  margin-bottom: 12px;
  font-weight: 600;
}

.add-course-form form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.add-course-form input {
  padding: 8px 12px;
  border-radius: 8px;
  border: 1px solid #ccc;
  font-size: 14px;
}

.form-buttons {
  display: flex;
  gap: 12px;
  justify-content: flex-end;
}

.submit-btn {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 12px;
  cursor: pointer;
}

.cancel-btn {
  background-color: #e0e0e0;
  border: none;
  padding: 8px 16px;
  border-radius: 12px;
  cursor: pointer;
}

/* Course Item Row */
.course-row {
  background: white;
  border-radius: 14px;
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.course-left {
  display: flex;
  align-items: center;
  gap: 14px;
}

.icon-box {
  width: 40px;
  height: 40px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon-box img {
  width: 24px;
  height: 24px;
}

.course-title {
  font-weight: 600;
  margin: 0 0 4px 0;
  font-size: 14px;
}

.author {
  display: flex;
  align-items: center;
  font-size: 12px;
  color: #6b7280;
}

.author img {
  width: 20px;
  height: 20px;
  border-radius: 9999px;
  margin-right: 6px;
}

/* Right Part */
.course-right {
  display: flex;
  align-items: center;
  gap: 24px;
}

.remaining-text {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.remaining-text .label {
  font-size: 12px;
  color: #9ca3af;
}

.remaining-text .time {
  font-weight: 600;
  font-size: 14px;
}

/* Circular Progress */
.progress-circle {
  position: relative;
  width: 36px;
  height: 36px;
}

.progress-circle svg {
  width: 100%;
  height: 100%;
  transform: rotate(-90deg);
}

.progress-circle .bg {
  fill: none;
  stroke: #eee;
  stroke-width: 3;
}

.progress-circle .fg {
  fill: none;
  stroke: #b0ec47;
  stroke-width: 3;
  stroke-linecap: round;
  transition: stroke-dasharray 0.3s ease;
}

.progress-circle .percent {
  position: absolute;
  top: 50%;
  left: 50%;
  font-size: 10px;
  font-weight: 600;
  transform: translate(-50%, -50%);
}
</style>
