<template>
  <div class="assignments-container">
    <div class="assignments-header">
      <h3 class="assignments-title">Assignments</h3>

      <div class="header-actions">
        <button class="dropdown" @click="toggleDropdown">{{ filter }} ▼</button>
        <button class="assignments-add" @click="showForm = true">+</button>
      </div>
    </div>

    <!-- Dropdown menu -->
    <div v-if="dropdownOpen" class="dropdown-menu">
      <div class="dropdown-item" @click="setFilter('All')">All</div>
      <div class="dropdown-item" @click="setFilter('In progress')">In progress</div>
      <div class="dropdown-item" @click="setFilter('Completed')">Completed</div>
      <div class="dropdown-item" @click="setFilter('Upcoming')">Upcoming</div>
    </div>

    <!-- Add Assignment Form -->
    <div v-if="showForm" class="add-assignment-form">
      <h4>Add New Assignment</h4>
      <form @submit.prevent="addAssignment">
        <input v-model="newAssignment.title" placeholder="Assignment Title" required />
        <input v-model="newAssignment.date" placeholder="Date (e.g. 20 June, 3:00 PM)" required />
        <select v-model="newAssignment.status" required>
          <option disabled value="">Select Status</option>
          <option>In progress</option>
          <option>Completed</option>
          <option>Upcoming</option>
        </select>
        <div class="form-buttons">
          <button type="submit" class="submit-btn">Add</button>
          <button type="button" class="cancel-btn" @click="cancelForm">Cancel</button>
        </div>
      </form>
    </div>

    <div
      v-for="(a, i) in filteredAssignments"
      :key="i"
      class="assignment-item"
    >
      <div class="left-section">
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
import { ref, reactive, computed } from 'vue'

const assignments = reactive([
  {
    title: 'Methods of data',
    date: '02 July, 10:30 AM',
    status: 'In progress',
  },
  {
    title: 'Market Research',
    date: '14 June, 12:45 AM',
    status: 'Completed',
  },
  {
    title: 'Data Collection',
    date: '12 May, 11:00 AM',
    status: 'Upcoming',
  }
])

const showForm = ref(false)
const dropdownOpen = ref(false)
const filter = ref('All')

const newAssignment = reactive({
  title: '',
  date: '',
  status: '',
})

function addAssignment() {
  assignments.push({
    title: newAssignment.title,
    date: newAssignment.date,
    status: newAssignment.status,
  })
  resetForm()
  showForm.value = false
}

function cancelForm() {
  resetForm()
  showForm.value = false
}

function resetForm() {
  newAssignment.title = ''
  newAssignment.date = ''
  newAssignment.status = ''
}

function toggleDropdown() {
  dropdownOpen.value = !dropdownOpen.value
}

function setFilter(value) {
  filter.value = value
  dropdownOpen.value = false
}

const filteredAssignments = computed(() => {
  if (filter.value === 'All') {
    return assignments
  }
  return assignments.filter(a => a.status === filter.value)
})

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
  position: relative;
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

.header-actions {
  display: flex;
  align-items: center;
  gap: 8px;
  position: relative;
}

.dropdown {
  background: white;
  border: 1px solid #ccc;
  border-radius: 16px;
  padding: 4px 10px;
  font-size: 14px;
  cursor: pointer;
  user-select: none;
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

/* Dropdown menu */
.dropdown-menu {
  position: absolute;
  top: 40px;
  right: 30px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 8px rgb(0 0 0 / 0.1);
  width: 140px;
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

/* Add Assignment Form Styles */
.add-assignment-form {
  background: white;
  border-radius: 14px;
  padding: 16px;
  margin-bottom: 16px;
}

.add-assignment-form h4 {
  margin-bottom: 12px;
  font-weight: 600;
  font-size: 16px;
}

.add-assignment-form form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.add-assignment-form input,
.add-assignment-form select {
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

/* Assignments List */
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
