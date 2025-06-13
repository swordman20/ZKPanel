<template>
  <div class="revenue-container">
    <div class="revenue-item">
      <div class="revenue-label">今日营收</div>
      <div class="revenue-value digital-text">{{ formatCurrency(todayRevenue) }}</div>
      <div class="revenue-unit">元</div>
    </div>
    <div class="date-info">
      <div class="date-item">{{ year }}</div>
      <div class="date-item">{{ month }}</div>
      <div class="date-item">{{ day }}</div>
      <div class="date-item">{{ hour }}</div>
      <div class="date-item">{{ minute }}</div>
    </div>
    <div class="revenue-item">
      <div class="revenue-label">本月营收</div>
      <div class="revenue-value digital-text">{{ formatCurrency(monthRevenue) }}</div>
      <div class="revenue-unit">元</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

// 收入数据
const todayRevenue = ref(58000)
const monthRevenue = ref(2230000)

// 时间
const year = ref('')
const month = ref('')
const day = ref('')
const hour = ref('')
const minute = ref('')

let timer = null

// 格式化货币，增加千分位分隔符
const formatCurrency = (value) => {
  return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
}

// 更新时间
const updateTime = () => {
  const now = new Date()
  year.value = now.getFullYear()
  month.value = String(now.getMonth() + 1).padStart(2, '0')
  day.value = String(now.getDate()).padStart(2, '0')
  hour.value = String(now.getHours()).padStart(2, '0')
  minute.value = String(now.getMinutes()).padStart(2, '0')
}

onMounted(() => {
  updateTime()
  timer = setInterval(updateTime, 60000) // 每分钟更新一次时间
})

onBeforeUnmount(() => {
  if (timer) {
    clearInterval(timer)
  }
})
</script>

<style lang="less" scoped>
.revenue-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 80px;
  margin-bottom: 10px;
}

.revenue-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: rgba(9, 31, 60, 0.6);
  border: 1px solid var(--border-color);
  border-radius: 4px;
  padding: 8px 15px;
  width: 30%;
  height: 100%;
}

.revenue-label {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 5px;
}

.revenue-value {
  font-size: 32px;
  line-height: 1;
  margin-bottom: 2px;
}

.revenue-unit {
  font-size: 14px;
  color: var(--text-color);
}

.date-info {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 35%;
  height: 100%;
  background-color: rgba(9, 31, 60, 0.6);
  border: 1px solid var(--border-color);
  border-radius: 4px;
  padding: 0 10px;
}

.date-item {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40px;
  height: 40px;
  background-color: rgba(6, 20, 40, 0.6);
  border: 1px solid var(--border-color);
  border-radius: 4px;
  font-family: var(--font-digital);
  font-size: 20px;
  color: var(--text-color);
}
</style> 