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

    <!-- D3 Bar Chart -->
    <div ref="d3Chart" class="hours-bars-d3"></div>

    <!-- Tooltip -->
    <div ref="tooltip" class="tooltip"></div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import * as d3 from 'd3'

const d3Chart = ref(null)
const tooltip = ref(null)

const days = ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa']
const data = [
  { value: 3.25, date: '2 Jan 2023' },
  { value: 4.10, date: '3 Jan 2023' },
  { value: 2.00, date: '4 Jan 2023' },
  { value: 6.75, date: '5 Jan 2023' },
  { value: 5.50, date: '6 Jan 2023' },
  { value: 1.10, date: '7 Jan 2023' },
  { value: 4.83, date: '8 Jan 2023' }
]

onMounted(() => {
  const width = 350
  const height = 180
  const margin = { top: 10, right: 10, bottom: 30, left: 10 }

  const svg = d3.select(d3Chart.value)
    .append('svg')
    .attr('width', width)
    .attr('height', height)

  const barWidth = 24
  const gap = 18
  const totalBarWidth = data.length * (barWidth + gap) - gap
  const startX = (width - totalBarWidth) / 2

  const maxVal = d3.max(data, d => d.value)
  const yScale = d3.scaleLinear()
    .domain([0, maxVal])
    .range([0, height - margin.bottom - 20])

  const tip = d3.select(tooltip.value)

  // Draw bars (all same color, no highlight)
  svg.selectAll('rect')
    .data(data)
    .enter()
    .append('rect')
    .attr('x', (d, i) => startX + i * (barWidth + gap))
    .attr('y', d => height - margin.bottom - yScale(d.value))
    .attr('width', barWidth)
    .attr('height', d => yScale(d.value))
    .attr('fill', '#1f2937')  // same color for all bars
    .attr('rx', 6)
    .on('mouseover', function (event, d) {
      d3.select(this).attr('fill', '#10b981')
      tip.style('display', 'block').html(`${d.value} hrs`)
    })
    .on('mousemove', (event) => {
      const containerRect = d3Chart.value.getBoundingClientRect()
      tip.style('left', (event.clientX - containerRect.left + 12) + 'px')
        .style('top', (event.clientY - containerRect.top - 28) + 'px')
    })
    .on('mouseout', function () {
      d3.select(this).attr('fill', '#1f2937')  // reset color
      tip.style('display', 'none')
    })

  // Draw day labels below bars
  svg.selectAll('text.day-label')
    .data(days)
    .enter()
    .append('text')
    .attr('class', 'day-label')
    .attr('x', (d, i) => startX + i * (barWidth + gap) + barWidth / 2)
    .attr('y', height - 8)
    .attr('text-anchor', 'middle')
    .attr('font-size', '12px')
    .attr('fill', '#6b7280')
    .text(d => d)
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
  position: relative;
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
  height: 180px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  position: relative;
}

/* Tooltip that follows the mouse */
.tooltip {
  position: absolute;
  display: none;
  background: #ffffff;
  color: #111827;
  font-size: 12px;
  padding: 6px 10px;
  border-radius: 6px;
  border: 1px solid #d1d5db;
  pointer-events: none;
  box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
  z-index: 999;
  white-space: nowrap;
}
</style>
