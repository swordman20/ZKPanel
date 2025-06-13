<template>
  <div class="map-container panel-box">
    <div class="panel-header">
      <div class="panel-title">飞行监控地图</div>
    </div>
    <div class="panel-content">
      <div id="map-view" class="map-view"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import AMapLoader from '@amap/amap-jsapi-loader'

const map = ref(null)
const markers = ref([])
const paths = ref([])

// 模拟的eVTOL飞行器数据点
const droneData = [
  { 
    id: 1, 
    name: 'eVTOL-001', 
    lat: 25.716820, 
    lng: 100.254789, 
    status: 1, 
    passengers: 4,
    route: '飞行路线1',
    flightStatus: '巡航中',
    dailyFlights: 8,
    batteryRemaining: 60,
    dailyDistance: 140,
    dailyIncome: 12000
  },
  { 
    id: 2, 
    name: 'eVTOL-002', 
    lat: 25.698427, 
    lng: 100.228354, 
    status: 1, 
    passengers: 6,
    route: '飞行路线2',
    flightStatus: '起飞中',
    dailyFlights: 6,
    batteryRemaining: 85,
    dailyDistance: 120,
    dailyIncome: 10800
  },
  { 
    id: 3, 
    name: 'eVTOL-003', 
    lat: 25.723456, 
    lng: 100.245678, 
    status: 2, 
    passengers: 2,
    route: '飞行路线3',
    flightStatus: '警告状态',
    dailyFlights: 5,
    batteryRemaining: 45,
    dailyDistance: 90,
    dailyIncome: 8500
  },
  { 
    id: 4, 
    name: 'eVTOL-004', 
    lat: 25.693215, 
    lng: 100.261532, 
    status: 1, 
    passengers: 4,
    route: '飞行路线4',
    flightStatus: '降落中',
    dailyFlights: 7,
    batteryRemaining: 35,
    dailyDistance: 130,
    dailyIncome: 11500
  },
  { 
    id: 5, 
    name: 'eVTOL-005', 
    lat: 25.705421, 
    lng: 100.215824, 
    status: 3, 
    passengers: 2,
    route: '飞行路线5',
    flightStatus: '异常状态',
    dailyFlights: 3,
    batteryRemaining: 20,
    dailyDistance: 60,
    dailyIncome: 5400
  }
]

// 模拟的飞行路线
const flightPaths = [
  {
    id: 1,
    path: [
      [100.254789, 25.716820], // 起点 eVTOL-001
      [100.262478, 25.721354],
      [100.269852, 25.725431],
      [100.279254, 25.719876]  // 终点
    ],
    status: 1
  },
  {
    id: 2,
    path: [
      [100.228354, 25.698427], // 起点 eVTOL-002
      [100.237421, 25.707272], // 经过中心点
      [100.245678, 25.723456]  // 终点 (eVTOL-003的位置)
    ],
    status: 1
  },
  {
    id: 3,
    path: [
      [100.215824, 25.705421], // 起点 eVTOL-005
      [100.225126, 25.712834],
      [100.234987, 25.719547],
      [100.245678, 25.723456]  // 终点 (eVTOL-003的位置)
    ],
    status: 2
  },
  {
    id: 4,
    path: [
      [100.261532, 25.693215], // 起点 eVTOL-004
      [100.252678, 25.689754],
      [100.242345, 25.695432],
      [100.237421, 25.707272]  // 终点 (中心点)
    ],
    status: 1
  }
]

// 添加eVTOL飞行器标记
const addDroneMarkers = (AMap) => {
  // 创建信息窗口但不绑定到标记上，稍后绑定到飞机图标上
  const infoWindows = droneData.map(drone => {
    return {
      drone,
      infoWindow: new AMap.InfoWindow({
        content: `<div class="info-window">
                    <div class="info-header">
                      <div class="info-title">${drone.name}</div>
                      <div class="info-status status-${drone.status === 1 ? 'normal' : drone.status === 2 ? 'warning' : 'danger'}">${getStatusText(drone.status)}</div>
                    </div>
                    <div class="info-divider"></div>
                    <div class="info-content">
                      <div class="info-row">
                        <div class="info-item">
                          <div class="info-label">飞行器名称：</div>
                          <div class="info-value">${drone.name}</div>
                        </div>
                        <div class="info-item">
                          <div class="info-label">飞行路线：</div>
                          <div class="info-value">${drone.route}</div>
                        </div>
                      </div>
                      <div class="info-row">
                        <div class="info-item">
                          <div class="info-label">飞行状态：</div>
                          <div class="info-value">${drone.flightStatus}</div>
                        </div>
                        <div class="info-item">
                          <div class="info-label">今日飞行架次：</div>
                          <div class="info-value">${drone.dailyFlights}次</div>
                        </div>
                      </div>
                      <div class="info-row">
                        <div class="info-item">
                          <div class="info-label">剩余电量：</div>
                          <div class="info-value">${drone.batteryRemaining}%</div>
                        </div>
                        <div class="info-item">
                          <div class="info-label">今日飞行距离：</div>
                          <div class="info-value">${drone.dailyDistance}km</div>
                        </div>
                      </div>
                      <div class="info-row">
                        <div class="info-item">
                          <div class="info-label">当前乘客数：</div>
                          <div class="info-value">${drone.passengers}人</div>
                        </div>
                        <div class="info-item">
                          <div class="info-label">今日营业收入：</div>
                          <div class="info-value">${drone.dailyIncome}元</div>
                        </div>
                      </div>
                      <div class="info-row">
                        <div class="info-item">
                          <div class="info-label">坐标：</div>
                          <div class="info-value">${drone.lng.toFixed(6)}, ${drone.lat.toFixed(6)}</div>
                        </div>
                      </div>
                    </div>
                    <div class="info-footer">
                      <button class="info-btn">查看详情</button>
                    </div>
                  </div>`,
        offset: new AMap.Pixel(0, -30),
        isCustom: true
      })
    }
  })
  
  // 保存信息窗口对象供其他函数使用
  return infoWindows
}

// 添加飞行路线
const addFlightPaths = (AMap, infoWindows) => {
  flightPaths.forEach((path, index) => {
    const polyline = new AMap.Polyline({
      path: path.path,
      strokeColor: getPathColorByStatus(path.status),
      strokeWeight: 3,
      strokeOpacity: 0.8,
      showDir: true,
      extData: path
    })
    
    paths.value.push(polyline)
    map.value.add(polyline)
    
    // 创建飞机图标（起点）
    const startMarkerContent = `
      <div class="custom-marker aircraft-marker">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24">
          <path fill="#00aaff" d="M22,16v-2l-8.5-5V3.5C13.5,2.67,12.83,2,12,2s-1.5,0.67-1.5,1.5V9L2,14v2l8.5-2.5V19L8,20.5V22l4-1l4,1v-1.5L13.5,19 v-5.5L22,16z" />
        </svg>
      </div>
    `
    
    // 创建停机坪图标（终点）
    const endMarkerContent = `
      <div class="custom-marker helipad-marker">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24">
          <circle fill="rgba(77, 179, 255, 0.2)" cx="12" cy="12" r="10" />
          <path fill="#00aaff" d="M12,2C6.48,2,2,6.48,2,12s4.48,10,10,10s10-4.48,10-10S17.52,2,12,2z M12,20c-4.42,0-8-3.58-8-8 s3.58-8,8-8s8,3.58,8,8S16.42,20,12,20z" />
          <path fill="#00aaff" d="M8,16h8v-2H8V16z M8,12h2V7h-2V12z M14,7v5h2V7H14z" />
        </svg>
      </div>
    `
    
    // 添加起点标记（飞机图标）
    const startMarker = new AMap.Marker({
      position: path.path[0],
      content: startMarkerContent,
      offset: new AMap.Pixel(-12, -12),
      zIndex: 100,
      extData: {
        pathId: path.id,
        isStart: true
      }
    })
    
    // 添加终点标记（停机坪图标）
    const endMarker = new AMap.Marker({
      position: path.path[path.path.length - 1],
      content: endMarkerContent,
      offset: new AMap.Pixel(-12, -12),
      zIndex: 100,
      extData: {
        pathId: path.id,
        isEnd: true
      }
    })
    
    map.value.add([startMarker, endMarker])
    markers.value.push(startMarker, endMarker)
    
    // 为起点飞机图标添加点击事件
    if (index < infoWindows.length) {
      startMarker.on('click', () => {
        infoWindows[index].infoWindow.open(map.value, startMarker.getPosition())
      })
    }
  })
}

// 初始化地图
const initMap = async () => {
  try {
    const AMap = await AMapLoader.load({
      key: '88f8067596e552e821541f3ff0b550aa', // 高德地图 API key
      version: '2.0',
      plugins: [
        'AMap.Scale',
        'AMap.ToolBar',
        'AMap.MapType'
      ]
    })
    
    map.value = new AMap.Map('map-view', {
      zoom: 12,
      center: [100.237421, 25.707272], // 默认中心点
      mapStyle: 'amap://styles/dark', // 暗色主题
      viewMode: '3D'
    })
    
    // 添加控件
    map.value.addControl(new AMap.Scale())
    map.value.addControl(new AMap.ToolBar())
    map.value.addControl(new AMap.MapType())
    
    // 创建信息窗口但不添加eVTOL标记
    const infoWindows = addDroneMarkers(AMap)
    
    // 添加飞行路线和标记，并将信息窗口绑定到飞机图标
    addFlightPaths(AMap, infoWindows)

    // 添加点击地图关闭信息窗口的事件监听
    map.value.on('click', () => {
      // 关闭所有信息窗口
      map.value.clearInfoWindow()
    })

    // 修改地图控件样式
    modifyMapControlStyle()
    
  } catch (e) {
    console.error('地图加载失败', e)
  }
}

// 修改地图控件样式
const modifyMapControlStyle = () => {
  // 在样式已完全加载后执行
  setTimeout(() => {
    const layerItems = document.querySelectorAll('.amap-ctrl-list-layer')
    if (layerItems && layerItems.length > 0) {
      layerItems.forEach(item => {
        item.style.backgroundColor = 'transparent'
        const spans = item.querySelectorAll('span')
        if (spans && spans.length > 0) {
          spans.forEach(span => {
            span.style.color = '#ffffff'
          })
        }
      })
    }
  }, 500)
}

// 根据状态获取图标
const getIconByStatus = (status) => {
  switch (status) {
    case 1: // 正常
      return 'https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png'
    case 2: // 警告
      return 'https://webapi.amap.com/theme/v1.3/markers/n/mark_y.png'
    case 3: // 异常
      return 'https://webapi.amap.com/theme/v1.3/markers/n/mark_r.png'
    default:
      return 'https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png'
  }
}

// 根据状态获取路径颜色
const getPathColorByStatus = (status) => {
  switch (status) {
    case 1: // 正常
      return '#00aaff'
    case 2: // 警告
      return '#ffcc00'
    default:
      return '#00aaff'
  }
}

// 获取状态文本
const getStatusText = (status) => {
  switch (status) {
    case 1: return '正常飞行'
    case 2: return '警告状态'
    case 3: return '异常状态'
    default: return '未知状态'
  }
}

onMounted(() => {
  initMap()
})

onUnmounted(() => {
  if (map.value) {
    map.value.destroy()
  }
})
</script>

<style lang="less" scoped>
.map-container {
  height: calc(100% - 140px);
}

.map-view {
  width: 100%;
  height: 100%;
  border-radius: 4px;
}

:deep(.info-window) {
  padding: 0;
  width: 380px;
  border-radius: 4px;
  overflow: hidden;
  font-family: "Microsoft YaHei", Arial, sans-serif;
  
  .info-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 15px;
    background-color: rgba(13, 39, 73, 0.9);
    
    .info-title {
      font-size: 16px;
      font-weight: bold;
      color: #fff;
    }
    
    .info-status {
      padding: 2px 8px;
      border-radius: 10px;
      font-size: 12px;
      font-weight: bold;
      
      &.status-normal {
        background-color: rgba(0, 255, 0, 0.2);
        color: #00ff00;
      }
      
      &.status-warning {
        background-color: rgba(255, 204, 0, 0.2);
        color: #ffcc00;
      }
      
      &.status-danger {
        background-color: rgba(255, 77, 79, 0.2);
        color: #ff4d4f;
      }
    }
  }
  
  .info-divider {
    height: 1px;
    background-color: rgba(255, 255, 255, 0.1);
  }
  
  .info-content {
    padding: 10px 15px;
    background-color: rgba(13, 39, 73, 0.8);
    
    .info-row {
      display: flex;
      margin-bottom: 8px;
      
      &:last-child {
        margin-bottom: 0;
      }
    }
    
    .info-item {
      flex: 1;
      display: flex;
      
      &:first-child {
        margin-right: 10px;
      }
    }
    
    .info-label {
      color: rgba(255, 255, 255, 0.7);
      font-size: 13px;
      white-space: nowrap;
    }
    
    .info-value {
      color: var(--text-color);
      font-size: 13px;
      font-weight: bold;
    }
  }
  
  .info-footer {
    padding: 8px 15px;
    background-color: rgba(9, 31, 60, 0.9);
    display: flex;
    justify-content: flex-end;
    
    .info-btn {
      padding: 4px 12px;
      background-color: var(--highlight-color);
      color: #fff;
      border: none;
      border-radius: 3px;
      font-size: 12px;
      cursor: pointer;
      transition: all 0.3s;
      
      &:hover {
        background-color: darken(#4db3ff, 10%);
      }
    }
  }
}

:deep(.amap-marker-label) {
  border: none;
  background-color: rgba(0, 0, 0, 0.7);
  color: #fff;
  padding: 3px 6px;
  border-radius: 2px;
}

/* 自定义标记样式 */
:deep(.custom-marker) {
  position: relative;
  width: 24px;
  height: 24px;
  
  svg {
    filter: drop-shadow(0 0 5px rgba(0, 170, 255, 0.7));
  }
  
  &.aircraft-marker {
    animation: pulse 1.5s infinite alternate;
    transform-origin: center;
  }
  
  &.helipad-marker {
    animation: glow 2s infinite alternate;
  }
}

@keyframes pulse {
  0% {
    transform: scale(1);
    opacity: 1;
  }
  100% {
    transform: scale(1.15);
    opacity: 0.8;
  }
}

@keyframes glow {
  0% {
    filter: brightness(1);
  }
  100% {
    filter: brightness(1.4);
  }
}

/* 高德地图控件样式重写 */
:deep(.amap-maps) {
  .amap-ctrl-list-layer {
    background-color: transparent !important;
    
    span {
      color: #fff !important;
      text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.8);
    }
  }
  
  .amap-layer-labels {
    z-index: 200 !important;
  }
  
  .amap-maptype-list {
    background-color: rgba(13, 39, 73, 0.8) !important;
    border: 1px solid var(--border-color) !important;
    
    .amap-maptype-list-item {
      background-color: transparent !important;
      color: #fff !important;
      
      &:hover {
        background-color: rgba(29, 69, 132, 0.8) !important;
      }
    }
  }
  
  .amap-toolbar {
    background-color: rgba(13, 39, 73, 0.8) !important;
    border: 1px solid var(--border-color) !important;
    border-radius: 4px;
    padding: 1px !important;
    
    .amap-toolbar-control {
      color: #fff !important;
      
      &:hover {
        color: var(--text-color) !important;
        background-color: rgba(29, 69, 132, 0.8) !important;
      }
    }
  }
  
  .amap-touch-toolbar {
    .amap-zoomcontrol {
      background-color: rgba(13, 39, 73, 0.8) !important;
      border: 1px solid var(--border-color) !important;
      border-radius: 4px;
      
      .amap-zoom-touch-plus, .amap-zoom-touch-minus {
        color: #fff !important;
        
        &:hover {
          color: var(--text-color) !important;
          background-color: rgba(29, 69, 132, 0.8) !important;
        }
      }
    }
  }
}
</style> 