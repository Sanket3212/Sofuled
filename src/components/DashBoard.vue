<template>
  <div class="dashboard-container">
    <!-- Hamburger button -->
    <button class="hamburger" @click="toggleSidebar">
      ☰
    </button>

    <!-- Sidebar -->
    <aside :class="['sidebar', { 'sidebar-hidden': !sidebarOpen && isMobile }]">
      <h1 class="sidebar-title">EDUCO</h1>
      <SidebarItem label="Dashboard" active />
      <SidebarItem label="My Courses" />
      <SidebarItem label="My Classes" />
      <SidebarItem label="Messages" />
      <SidebarItem label="Notifications" badge="2" />
      <SidebarItem label="Calendars" />
      <SidebarItem label="Community" />
      <SidebarItem label="Settings" />
    </aside>

    <!-- Main content -->
    <main class="content-wrapper">
      <!-- Header -->
      <div class="header">
        <h2>Welcome back Taylor 👋</h2>
        <div class="header-right">
          <input type="text" placeholder="Search courses" />
          <img src="https://i.pravatar.cc/40" />
        </div>
      </div>

      <!-- Responsive Grid -->
      <div class="main-grid">
        <!-- Main Column -->
        <div class="main-column">
          <CourseCard />
          <div class="grid-2-row">
            <ActivityChart />
            <ScheduleList />
          </div>
          <div class="spacer"></div>
          <CourseProgress />
        </div>

        <!-- Right Column -->
        <div class="right-column">
          <div class="card">
            <div class="calendar-header">
              <h3>August, 2023</h3>
              <div>
                <button>&lt;</button>
                <button>&gt;</button>
              </div>
            </div>
            <Calendar />
          </div>
          <div class="card">
            <Assignments />
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import SidebarItem from './SidebarItem.vue'
import CourseCard from './CourseCard.vue'
import Calendar from './CalendarView.vue'
import ActivityChart from './ActivityChart.vue'
import ScheduleList from './ScheduleList.vue'
import Assignments from './AssignmentsView.vue'
import CourseProgress from './CourseProgress.vue'

const sidebarOpen = ref(true)
const isMobile = ref(false)

const checkIsMobile = () => {
  isMobile.value = window.innerWidth < 1024
  if (isMobile.value) sidebarOpen.value = false
  else sidebarOpen.value = true
}

onMounted(() => {
  checkIsMobile()
  window.addEventListener('resize', checkIsMobile)
})
function toggleSidebar() {
  sidebarOpen.value = !sidebarOpen.value
}
</script>

<style scoped>
/* Fonts and Variables */
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap');

:root {
  --primary: #d3f365;
  --background: #f9f9f9;
  --text-primary: #1d1d1d;
  --text-secondary: #6b7280;
  --card-bg: #ffffff;
  --sidebar-bg: linear-gradient(180deg, #1e1f2b, #13141c);
  --font-main: 'Plus Jakarta Sans', sans-serif;
}

body,
html {
  margin: 0;
  padding: 0;
  font-family: var(--font-main);
  background-color: var(--background);
  height: 100%;
}

.dashboard-container {
  display: flex;
  height: 100vh;
  overflow: hidden;
  position: relative;
}

/* Hamburger */
.hamburger {
  position: fixed;
  top: 16px;
  left: 16px;
  font-size: 24px;
  background: rgb(14, 18, 105);
  color: white;
  border: none;
  border-radius: 6px;
  padding: 4px 10px;
  z-index: 1001;
  display: none;
}

/* Sidebar */
.sidebar {
  width: 220px;
  background: rgb(14, 18, 105);
  color: white;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  transition: transform 0.3s ease-in-out;
  z-index: 1000;
}

.sidebar-title {
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 24px;
  letter-spacing: 1px;
}

.sidebar-hidden {
  transform: translateX(-100%);
  position: fixed;
  height: 100%;
  top: 0;
  left: 0;
}

/* Main Area */
.content-wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  height: 100vh;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 16px 12px;
  background-color: var(--background);
}

.header h2 {
  font-size: 22px;
  font-weight: 700;
  color: var(--text-primary);
}

.header-right {
  display: flex;
  gap: 12px;
  align-items: center;
}

.header input {
  padding: 6px 14px;
  border-radius: 9999px;
  border: none;
  font-size: 13px;
  font-weight: 400;
  box-shadow: 0 0 0 1px #e5e7eb;
  background-color: white;
}

.header img {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  border: 2px solid #ccc;
}

.main-grid {
  flex: 1;
  display: grid;
  grid-template-columns: 1fr 320px;
  gap: 16px;
  padding: 0 16px 16px;
  overflow-y: auto;
}

.main-column {
  display: flex;
  flex-direction: column;
  gap: 16px;
  min-height: 100%;
}

.spacer {
  flex: 1;
}

.right-column {
  width: 320px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.card {
  background-color: var(--card-bg);
  border-radius: 12px;
  padding: 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  width: 100%;
  box-sizing: border-box;
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.grid-2-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

/* Responsive */
@media (max-width: 1024px) {
  .hamburger {
    display: block;
  }

  .main-grid {
    display: flex;
    flex-direction: column;
  }

  .main-column,
  .right-column {
    width: 100%;
  }

  .grid-2-row {
    grid-template-columns: 1fr;
  }

  .header {
    flex-direction: column;
    align-items: flex-start;
    padding-top: 60px;
  }

  .header-right {
    width: 100%;
    justify-content: space-between;
  }
}

@media (max-width: 600px) {
  .sidebar {
    position: fixed;
    top: 0;
    left: 0;
    height: 100vh;
    width: 220px;
  }

  .sidebar-hidden {
    transform: translateX(-100%);
  }

  .header h2 {
    font-size: 18px;
  }

  .header input {
    width: 100%;
  }
  
  

  .header-right {
    flex-direction: column;
    align-items: flex-start;
    gap: 6px;
  }

  .card {
    padding: 12px;
  }
  .header h2 {
    display: none;
  }
    .header h2 {
    display: none;
  }

  .header input {
    display: none;
  }

  .header img {
    display: none;
  }
}
</style>
