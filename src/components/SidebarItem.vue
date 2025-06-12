<template>
  <div
    class="sidebar-item"
    :class="{ active }"
  >
    <span>{{ label }}</span>
    <span v-if="badge" class="item-badge">{{ badge }}</span>
  </div>
</template>

<script setup>
// eslint-disable-next-line no-undef
defineProps({
  label: String,
  active: Boolean,
  badge: [String, Number]
})
</script>

<style scoped>
.sidebar-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 16px;
  font-size: 14px;
  border-radius: 10px;
  cursor: pointer;
  color: #cbd5e1;
  transition: 0.3s;
  /* background-color: #1e2230; */
}

.sidebar-item:hover {
  background-color: #ffffff;
  color: #000000;
}

.sidebar-item.active {
  background-color: white;
  color: black;
  position: relative;
}

.sidebar-item.active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  width: 4px;
  height: 50%;
  background-color: var(--primary);
  border-radius: 0 4px 4px 0;
  transform: translateY(-50%);
}

.item-badge {
  background-color: var(--primary);
  color: black;
  padding: 2px 10px;
  font-size: 12px;
  border-radius: 9999px;
}
</style>