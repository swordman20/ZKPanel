<template>
  <div class="flight-stats panel-box">
    <div class="panel-header">
      <div class="panel-title">飞行数据统计</div>
    </div>
    <div class="panel-content">
      <div class="stats-overview">
        <div class="stat-card">
          <div class="stat-icon">
            <IconFlight :size="36" color="#4db3ff" />
          </div>
          <div class="stat-info">
            <div class="stat-label">飞行架次</div>
            <div class="stat-value digital-text">{{ formatNumber(flightCount) }}</div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">
            <IconRoute :size="36" color="#4db3ff" />
          </div>
          <div class="stat-info">
            <div class="stat-label">飞行路线</div>
            <div class="stat-value digital-text">{{ formatNumber(routeCount) }}</div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">
            <IconArea :size="36" color="#4db3ff" />
          </div>
          <div class="stat-info">
            <div class="stat-label">累计飞行时长/时</div>
            <div class="stat-value digital-text">{{ formatNumber(workArea) }}</div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">
            <IconDrone :size="36" color="#4db3ff" />
          </div>
          <div class="stat-info">
            <div class="stat-label">eVTOL飞行器数量</div>
            <div class="stat-value digital-text">{{ formatNumber(droneCount) }}</div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">
            <IconOperator :size="36" color="#4db3ff" />
          </div>
          <div class="stat-info">
            <div class="stat-label">飞行员人数</div>
            <div class="stat-value digital-text">{{ formatNumber(operatorCount) }}</div>
          </div>
        </div>
      </div>
      
      <div class="flight-breakdown">
        <div class="breakdown-title">飞行架次明细</div>
        <div class="breakdown-content">
          <div class="breakdown-item">
            <div class="breakdown-label">类别一：民用</div>
            <div class="breakdown-value">
              <div class="breakdown-bar">
                <div class="bar-inner" :style="{ width: '16%' }"></div>
              </div>
              <div class="breakdown-number">34</div>
              <div class="breakdown-percent">16%</div>
            </div>
          </div>
          
          <div class="breakdown-item">
            <div class="breakdown-label">类别二：eVTOL-1</div>
            <div class="breakdown-value">
              <div class="breakdown-bar">
                <div class="bar-inner" :style="{ width: '28%' }"></div>
              </div>
              <div class="breakdown-number">100</div>
              <div class="breakdown-percent">28%</div>
            </div>
          </div>
          
          <div class="breakdown-item">
            <div class="breakdown-label">类别三：eVTOL-3</div>
            <div class="breakdown-value">
              <div class="breakdown-bar">
                <div class="bar-inner" :style="{ width: '28%' }"></div>
              </div>
              <div class="breakdown-number">100</div>
              <div class="breakdown-percent">28%</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import IconFlight from '@/components/icons/IconFlight.vue'
import IconRoute from '@/components/icons/IconRoute.vue'
import IconArea from '@/components/icons/IconArea.vue'
import IconDrone from '@/components/icons/IconDrone.vue'
import IconOperator from '@/components/icons/IconOperator.vue'

const flightCount = ref(635)
const routeCount = ref(88)
const workArea = ref(8548)
const droneCount = ref(6597)
const operatorCount = ref(15781)

// 格式化数字，增加千分位分隔符
const formatNumber = (value) => {
  return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
}
</script>

<style lang="less" scoped>
.flight-stats {
  height: 60%;
}

.stats-overview {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 15px;
  margin-bottom: 20px;
}

.stat-card {
  display: flex;
  align-items: center;
  background-color: rgba(13, 39, 73, 0.5);
  border: 1px solid rgba(29, 69, 132, 0.5);
  border-radius: 4px;
  padding: 10px;
}

.stat-icon {
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 10px;
  filter: drop-shadow(0 0 3px rgba(77, 179, 255, 0.7));
}

.stat-info {
  flex: 1;
}

.stat-label {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 5px;
}

.stat-value {
  font-size: 22px;
  color: var(--text-color);
}

.flight-breakdown {
  margin-top: 15px;
}

.breakdown-title {
  font-size: 14px;
  color: var(--text-color);
  margin-bottom: 15px;
  font-weight: bold;
}

.breakdown-item {
  margin-bottom: 15px;
}

.breakdown-label {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 5px;
}

.breakdown-value {
  display: flex;
  align-items: center;
}

.breakdown-bar {
  flex: 1;
  height: 6px;
  background-color: rgba(9, 31, 60, 0.5);
  border-radius: 3px;
  overflow: hidden;
  margin-right: 10px;
  
  .bar-inner {
    height: 100%;
    background-color: var(--text-color);
    box-shadow: 0 0 6px rgba(77, 179, 255, 0.8);
  }
}

.breakdown-number {
  font-size: 14px;
  color: #fff;
  margin-right: 10px;
  width: 40px;
  text-align: right;
}

.breakdown-percent {
  font-size: 14px;
  color: var(--text-color);
  width: 40px;
}
</style> 