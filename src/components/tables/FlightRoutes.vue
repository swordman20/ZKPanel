<template>
  <div class="flight-routes panel-box">
    <div class="panel-header">
      <div class="panel-title">飞行路线规划</div>
      <div class="panel-close" @click="closePanel">×</div>
    </div>
    <div class="panel-content">
      <div class="route-info">
        <div class="route-row route-header">
          <div class="route-col">飞行编号</div>
          <div class="route-col">飞行路线</div>
          <div class="route-col">巡航速度</div>
          <div class="route-col">高度</div>
        </div>
        <div class="route-content">
          <div v-for="route in routes" :key="route.id" class="route-row">
            <div class="route-col">{{ route.id }}</div>
            <div class="route-col">{{ route.name }}</div>
            <div class="route-col">{{ route.speed }} km/h</div>
            <div class="route-col">{{ route.altitude }} m</div>
          </div>
        </div>
      </div>
      <div class="route-detail">
        <div class="detail-item">
          <div class="detail-label">当前路线：</div>
          <div class="detail-value">飞行路线1</div>
        </div>
        <div class="detail-item">
          <div class="detail-label">路线总长：</div>
          <div class="detail-value">6.5km</div>
        </div>
        <div class="detail-item">
          <div class="detail-label">预计飞行时间：</div>
          <div class="detail-value">00:05:30</div>
        </div>
        <div class="detail-item">
          <div class="detail-label">今日完成次数：</div>
          <div class="detail-value">12</div>
        </div>
        <div class="detail-item">
          <div class="detail-label">今日载客人次：</div>
          <div class="detail-value">1200人</div>
        </div>
        <div class="detail-item">
          <div class="detail-label">每日营收：</div>
          <div class="detail-value">￥12000</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const routes = ref([
  { id: '飞行路线1', name: '苍山-洱海-崇圣寺', speed: 60, altitude: 150 },
  { id: '飞行路线2', name: '洱海-大理古城', speed: 70, altitude: 180 },
  { id: '飞行路线3', name: '苍山-洱海-喜洲', speed: 75, altitude: 200 },
  { id: '飞行路线4', name: '环洱海', speed: 65, altitude: 170 },
  { id: '飞行路线5', name: '大理古城-蝴蝶泉', speed: 55, altitude: 150 }
])

const closePanel = () => {
  // 关闭面板逻辑，可以通过事件通知父组件
  console.log('Close panel')
}
</script>

<style lang="less" scoped>
.flight-routes {
  height: 33%;
}

.panel-header {
  position: relative;
}

.panel-close {
  position: absolute;
  top: 0;
  right: 0;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(255, 255, 255, 0.7);
  cursor: pointer;
  font-size: 20px;
  
  &:hover {
    color: #fff;
  }
}

.route-info {
  margin-bottom: 15px;
}

.route-row {
  display: flex;
  align-items: center;
  padding: 6px 0;
  border-bottom: 1px solid rgba(29, 69, 132, 0.3);
  
  &.route-header {
    color: var(--text-color);
    font-weight: bold;
  }
}

.route-col {
  flex: 1;
  padding: 0 5px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-size: 12px;
  
  &:first-child {
    flex: 0.8;
  }
  
  &:nth-child(2) {
    flex: 1.5;
  }
}

.route-content {
  max-height: 150px;
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

.route-detail {
  padding: 5px 10px;
  background-color: rgba(13, 39, 73, 0.3);
  border-radius: 4px;
}

.detail-item {
  display: flex;
  margin-bottom: 5px;
  font-size: 12px;
}

.detail-label {
  width: 100px;
  color: rgba(255, 255, 255, 0.7);
}

.detail-value {
  flex: 1;
  color: var(--text-color);
}
</style> 