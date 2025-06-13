<template>
  <div class="drone-status panel-box">
    <div class="panel-header">
      <div class="panel-title">飞行器运行状况</div>
    </div>
    <div class="panel-content">
      <div class="status-table-wrapper">
        <table class="status-table">
          <thead>
            <tr>
              <th>No.</th>
              <th>飞行器</th>
              <th>持续飞行</th>
              <th>机身温度</th>
              <th>电池电量</th>
              <th>信号强度</th>
              <th>空速</th>
              <th>状态</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in droneList" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ item.name }}</td>
              <td>{{ item.duration }}</td>
              <td :class="getTemperatureClass(item.temperature)">{{ item.temperature }}°C</td>
              <td :class="getBatteryClass(item.battery)">{{ item.battery }}%</td>
              <td :class="getSignalClass(item.signal)">{{ item.signal }}%</td>
              <td>{{ item.speed }} m/s</td>
              <td :class="getStatusClass(item.status)">{{ getStatusText(item.status) }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const droneList = ref([
  { name: 'eVTOL-001', duration: '00:05:30', temperature: 42, battery: 68, signal: 90, speed: 6.11, status: 'normal' },
  { name: 'eVTOL-002', duration: '00:05:30', temperature: 38, battery: 75, signal: 95, speed: 6.11, status: 'normal' },
  { name: 'eVTOL-003', duration: '00:05:30', temperature: 39, battery: 60, signal: 80, speed: 6.12, status: 'normal' },
  { name: 'eVTOL-004', duration: '00:05:30', temperature: 45, battery: 45, signal: 85, speed: 6.12, status: 'warning' },
  { name: 'eVTOL-005', duration: '00:05:30', temperature: 47, battery: 35, signal: 70, speed: 6.12, status: 'danger' }
])

// 获取温度对应的样式类
const getTemperatureClass = (temp) => {
  if (temp >= 45) return 'status-danger'
  if (temp >= 42) return 'status-warning'
  return 'status-normal'
}

// 获取电池对应的样式类
const getBatteryClass = (battery) => {
  if (battery <= 30) return 'status-danger'
  if (battery <= 50) return 'status-warning'
  return 'status-normal'
}

// 获取信号强度对应的样式类
const getSignalClass = (signal) => {
  if (signal <= 50) return 'status-danger'
  if (signal <= 80) return 'status-warning'
  return 'status-normal'
}

// 获取状态对应的样式类
const getStatusClass = (status) => {
  if (status === 'danger') return 'status-danger'
  if (status === 'warning') return 'status-warning'
  return 'status-normal'
}

// 获取状态文本
const getStatusText = (status) => {
  if (status === 'danger') return '异常'
  if (status === 'warning') return '警告'
  return '正常'
}
</script>

<style lang="less" scoped>
.drone-status {
  height: 33%;
  margin: 10px 0;
}

.status-table-wrapper {
  width: 100%;
  max-height: calc(100% - 10px);
  overflow-y: auto;
  
  &::-webkit-scrollbar {
    width: 5px;
  }
  
  &::-webkit-scrollbar-thumb {
    background-color: rgba(77, 179, 255, 0.3);
    border-radius: 3px;
  }
  
  &::-webkit-scrollbar-track {
    background-color: rgba(9, 31, 60, 0.3);
  }
}

.status-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 12px;
  
  th, td {
    padding: 8px 5px;
    text-align: center;
    border: 1px solid rgba(29, 69, 132, 0.3);
  }
  
  th {
    background-color: rgba(29, 69, 132, 0.6);
    color: var(--text-color);
    font-weight: bold;
  }
  
  tr:nth-child(even) {
    background-color: rgba(13, 39, 73, 0.3);
  }
  
  tr:hover {
    background-color: rgba(29, 69, 132, 0.3);
  }
}

.status-normal {
  color: var(--success-color);
}

.status-warning {
  color: var(--warning-color);
}

.status-danger {
  color: var(--danger-color);
}
</style> 