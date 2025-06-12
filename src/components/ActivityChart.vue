<template>
  <div class="hours-activity-container">
    <div class="hours-header">
      <div>
        <h3 class="hours-title">Hours Activity</h3>
        <div class="hours-increase">
          <span class="arrow-up">↑</span>
          <span class="percent">+3%</span> Increase than last week
        </div>
      </div>
      <div class="dropdown">Weekly ⌄</div>
    </div>

    <!-- D3 Bar Chart Mount Point -->
    <div ref="d3Chart" class="hours-bars-d3"></div>

    <!-- Day Labels -->
    <div class="days-row">
      <span v-for="(d, i) in days" :key="i">{{ d }}</span>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import * as d3 from 'd3' // ✅ Import D3

const d3Chart = ref(null)

const days = ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa']
const data = [
  { value: 3.25, date: '2 Jan 2023' },
  { value: 4.10, date: '3 Jan 2023' },
  { value: 2.00, date: '4 Jan 2023' },
  { value: 6.75, date: '5 Jan 2023' }, // highest
  { value: 5.50, date: '6 Jan 2023' },
  { value: 1.10, date: '7 Jan 2023' },
  { value: 4.83, date: '8 Jan 2023' }
]

onMounted(() => {
  const width = 350
  const height = 160
  const margin = { top: 10, right: 10, bottom: 20, left: 10 }

  const svg = d3
    .select(d3Chart.value)
    .append('svg')
    .attr('width', width)
    .attr('height', height)

  const maxVal = d3.max(data, d => d.value)
  const yScale = d3.scaleLinear().domain([0, maxVal]).range([0, height - margin.bottom])

  const barWidth = 24
  const gap = 18

  const bars = svg
    .selectAll('rect')
    .data(data)
    .enter()
    .append('rect')
    .attr('x', (d, i) => i * (barWidth + gap))
    .attr('y', d => height - yScale(d.value))
    .attr('width', barWidth)
    .attr('height', d => yScale(d.value))
    .attr('fill', d => (d.value === maxVal ? '#c5ff55' : '#1f2937'))
    .attr('rx', 6)

  bars.append('title') // Tooltips on hover
    .text(d => `${d.value.toFixed(2)} hours\n${d.date}`)
})
</script>

<style scoped>
.hours-activity-container {
  background-color: #ffffff;
  padding: 16px;
  border-radius: 20px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  font-family: 'Inter', sans-serif;
  max-width: 350px;
}

.hours-header {
  display: flex;
  justify-content: space-between;
  align-items: start;
  margin-bottom: 16px;
}

.hours-title {
  font-size: 16px;
  font-weight: 600;
  color: #1d1d1d;
  margin-bottom: 4px;
}

.hours-increase {
  font-size: 12px;
  color: #16a34a;
  display: flex;
  align-items: center;
}

.arrow-up {
  font-size: 14px;
  margin-right: 4px;
}

.dropdown {
  background-color: #f3f4f6;
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 12px;
  color: #1f2937;
  cursor: pointer;
  user-select: none;
}

.hours-bars-d3 {
  height: 160px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  margin-bottom: 12px;
  overflow-x: auto;
}

.days-row {
  display: flex;
  justify-content: space-between;
  font-size: 13px;
  color: #6b7280;
}
</style>
