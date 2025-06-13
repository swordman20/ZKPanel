<template>
  <div class="flight-records panel-box">
    <div class="panel-header">
      <div class="panel-title">飞行器信息查询</div>
    </div>
    <div class="panel-content">
      <div class="filter-row">
        <div class="filter-title">飞行器</div>
        <div class="filter-options">
          <div class="filter-option" :class="{ active: currentFilter === 'all' }" @click="setFilter('all')">全部</div>
          <div class="filter-option" :class="{ active: currentFilter === 'normal' }" @click="setFilter('normal')">正常</div>
          <div class="filter-option" :class="{ active: currentFilter === 'warning' }" @click="setFilter('warning')">警告</div>
          <div class="filter-option" :class="{ active: currentFilter === 'offline' }" @click="setFilter('offline')">离线</div>
        </div>
        <div class="filter-search">
          <input type="text" placeholder="请输入关键字" v-model="searchKeyword" />
        </div>
      </div>
      <div class="records-table-wrapper">
        <table class="records-table">
          <thead>
            <tr>
              <th>No.</th>
              <th>飞行编号</th>
              <th>飞行器类型</th>
              <th>飞行员</th>
              <th>飞行时长</th>
              <th>载客情况</th>
              <th>开始时间</th>
              <th>完成情况</th>
              <th>飞行状态</th>
              <th>操作</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(record, index) in filteredRecords" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ record.flightNo }}</td>
              <td>{{ record.droneType }}</td>
              <td>{{ record.pilot }}</td>
              <td>{{ record.duration }}</td>
              <td>{{ record.payload }}</td>
              <td>{{ record.startTime }}</td>
              <td>{{ record.completion }}</td>
              <td :class="'status-' + record.status">{{ getStatusText(record.status) }}</td>
              <td class="operation-cell">
                <span class="op-btn">查看</span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const currentFilter = ref('all')
const searchKeyword = ref('')

const records = [
  { flightNo: '飞行编号1', droneType: '载客型', pilot: '张三', duration: '00:05:30', payload: '4人', startTime: '08:00', completion: '100%', status: 'normal' },
  { flightNo: '飞行编号2', droneType: '载客型', pilot: '李四', duration: '00:05:30', payload: '6人', startTime: '08:05', completion: '100%', status: 'normal' },
  { flightNo: '飞行编号3', droneType: 'eVTOL', pilot: '王五', duration: '00:05:30', payload: '2人', startTime: '08:10', completion: '95%', status: 'warning' },
  { flightNo: '飞行编号4', droneType: 'eVTOL', pilot: '赵六', duration: '00:05:30', payload: '4人', startTime: '08:15', completion: '80%', status: 'danger' },
  { flightNo: '飞行编号5', droneType: 'eVTOL', pilot: '钱七', duration: '00:05:30', payload: '2人', startTime: '08:20', completion: '90%', status: 'warning' }
]

// 筛选记录
const filteredRecords = computed(() => {
  let result = records
  
  // 根据状态筛选
  if (currentFilter.value !== 'all') {
    result = result.filter(record => record.status === currentFilter.value)
  }
  
  // 根据关键字筛选
  if (searchKeyword.value) {
    const keyword = searchKeyword.value.toLowerCase()
    result = result.filter(record => 
      record.flightNo.toLowerCase().includes(keyword) ||
      record.droneType.toLowerCase().includes(keyword) ||
      record.pilot.toLowerCase().includes(keyword)
    )
  }
  
  return result
})

// 设置筛选条件
const setFilter = (filter) => {
  currentFilter.value = filter
}

// 获取状态文本
const getStatusText = (status) => {
  if (status === 'danger') return '异常'
  if (status === 'warning') return '警告'
  if (status === 'offline') return '离线'
  return '正常'
}
</script>

<style lang="less" scoped>
.flight-records {
  height: calc(34% - 10px);
}

.filter-row {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  font-size: 12px;
}

.filter-title {
  color: var(--text-color);
  margin-right: 10px;
  font-weight: bold;
}

.filter-options {
  display: flex;
  margin-right: 20px;
}

.filter-option {
  padding: 3px 8px;
  margin-right: 5px;
  cursor: pointer;
  border-radius: 2px;
  color: rgba(255, 255, 255, 0.7);
  
  &:hover {
    color: var(--text-color);
  }
  
  &.active {
    background-color: rgba(29, 69, 132, 0.6);
    color: var(--text-color);
  }
}

.filter-search {
  flex: 1;
  
  input {
    width: 100%;
    height: 24px;
    background-color: rgba(13, 39, 73, 0.5);
    border: 1px solid rgba(29, 69, 132, 0.5);
    border-radius: 2px;
    color: #fff;
    padding: 0 10px;
    outline: none;
    
    &:focus {
      border-color: var(--text-color);
    }
    
    &::placeholder {
      color: rgba(255, 255, 255, 0.3);
    }
  }
}

.records-table-wrapper {
  width: 100%;
  max-height: calc(100% - 30px);
  overflow-y: auto;
  
  &::-webkit-scrollbar {
    width: 5px;
    height: 5px;
  }
  
  &::-webkit-scrollbar-thumb {
    background-color: rgba(77, 179, 255, 0.3);
    border-radius: 3px;
  }
  
  &::-webkit-scrollbar-track {
    background-color: rgba(9, 31, 60, 0.3);
  }
}

.records-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 12px;
  
  th, td {
    padding: 6px 4px;
    text-align: center;
    border: 1px solid rgba(29, 69, 132, 0.3);
  }
  
  th {
    background-color: rgba(29, 69, 132, 0.6);
    color: var(--text-color);
    font-weight: bold;
    white-space: nowrap;
  }
  
  td {
    white-space: nowrap;
  }
  
  tr:nth-child(even) {
    background-color: rgba(13, 39, 73, 0.3);
  }
  
  tr:hover {
    background-color: rgba(29, 69, 132, 0.3);
  }
}

.operation-cell {
  .op-btn {
    padding: 2px 5px;
    background-color: rgba(29, 69, 132, 0.6);
    color: var(--text-color);
    border-radius: 2px;
    cursor: pointer;
    
    &:hover {
      background-color: rgba(77, 179, 255, 0.3);
    }
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

.status-offline {
  color: #999;
}
</style> 