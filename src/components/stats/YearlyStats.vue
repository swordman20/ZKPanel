<template>
  <div class="yearly-stats panel-box">
    <div class="panel-header">
      <div class="panel-title">历年统计</div>
    </div>
    <div class="panel-content">
      <div class="chart-filter">
        <div class="filter-btn" :class="{ active: currentType === 'income' }" @click="switchType('income')">营收</div>
        <div class="filter-btn" :class="{ active: currentType === 'flights' }" @click="switchType('flights')">架次</div>
      </div>
      <div ref="chartContainer" class="chart-container"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, nextTick } from 'vue'
import * as echarts from 'echarts'

const chartContainer = ref(null)
const chart = ref(null)
const currentType = ref('income')

const incomeData = {
  years: ['2019', '2020', '2021', '2022', '2023', '2024'],
  values: [2737, 3787, 4327, 5407, 6327, 6876]
}

const flightsData = {
  years: ['2019', '2020', '2021', '2022', '2023', '2024'],
  values: [341, 432, 531, 612, 735, 862]
}

const initChart = () => {
  if (!chartContainer.value) return
  
  chart.value = echarts.init(chartContainer.value)
  updateChart()
  
  window.addEventListener('resize', () => {
    chart.value?.resize()
  })
}

const updateChart = () => {
  if (!chart.value) return
  
  const data = currentType.value === 'income' ? incomeData : flightsData
  
  const option = {
    grid: {
      top: 30,
      bottom: 50,
      left: 60,
      right: 30
    },
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'shadow'
      },
      formatter: (params) => {
        const param = params[0]
        return `${param.name}年: ${param.value} ${currentType.value === 'income' ? '万元' : '次'}`
      }
    },
    xAxis: {
      type: 'category',
      data: data.years,
      axisLine: {
        lineStyle: {
          color: 'rgba(255, 255, 255, 0.3)'
        }
      },
      axisLabel: {
        color: 'rgba(255, 255, 255, 0.7)'
      }
    },
    yAxis: {
      type: 'value',
      axisLine: {
        show: true,
        lineStyle: {
          color: 'rgba(255, 255, 255, 0.3)'
        }
      },
      splitLine: {
        lineStyle: {
          color: 'rgba(255, 255, 255, 0.1)'
        }
      },
      axisLabel: {
        color: 'rgba(255, 255, 255, 0.7)',
        formatter: (value) => {
          if (currentType.value === 'income') {
            return value.toFixed(0)
          }
          return value
        }
      }
    },
    series: [
      {
        data: data.values,
        type: 'line',
        smooth: true,
        symbol: 'emptyCircle',
        symbolSize: 8,
        lineStyle: {
          width: 3,
          color: {
            type: 'linear',
            x: 0,
            y: 0,
            x2: 0,
            y2: 1,
            colorStops: [
              { offset: 0, color: '#00BFFF' },
              { offset: 1, color: '#4db3ff' }
            ]
          }
        },
        itemStyle: {
          color: '#4db3ff',
          borderColor: '#fff',
          borderWidth: 1
        },
        areaStyle: {
          color: {
            type: 'linear',
            x: 0,
            y: 0,
            x2: 0,
            y2: 1,
            colorStops: [
              { offset: 0, color: 'rgba(77, 179, 255, 0.3)' },
              { offset: 1, color: 'rgba(77, 179, 255, 0.1)' }
            ]
          }
        }
      }
    ]
  }
  
  chart.value.setOption(option)
}

const switchType = (type) => {
  currentType.value = type
}

watch(currentType, () => {
  nextTick(() => {
    updateChart()
  })
})

onMounted(() => {
  initChart()
})
</script>

<style lang="less" scoped>
.yearly-stats {
  height: calc(40% - 10px);
  margin-top: 10px;
}

.chart-filter {
  display: flex;
  margin-bottom: 10px;
}

.filter-btn {
  padding: 5px 15px;
  font-size: 12px;
  color: rgba(255, 255, 255, 0.7);
  background-color: rgba(9, 31, 60, 0.5);
  border: 1px solid rgba(29, 69, 132, 0.5);
  border-radius: 2px;
  cursor: pointer;
  margin-right: 10px;
  
  &:hover {
    color: var(--text-color);
  }
  
  &.active {
    background-color: rgba(29, 69, 132, 0.7);
    color: var(--text-color);
    border-color: var(--text-color);
  }
}

.chart-container {
  width: 100%;
  height: calc(100% - 35px);
}
</style> 