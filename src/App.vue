<template>
  <div class="dashboard-container">
    <div class="dashboard-header">
      <div class="dashboard-title">中空领航飞行运营数据驾驶舱</div>
    </div>
    
    <div class="dashboard-content">
      <div class="left-panel">
        <FlightStats />
        <YearlyStats />
      </div>
      
      <div class="center-panel">
        <LocationHeader />
        <RevenueStats />
        <MapContainer />
      </div>
      
      <div class="right-panel">
        <FlightRoutes />
        <DroneStatus />
        <FlightRecords />
      </div>
    </div>
  </div>
</template>

<script setup>
import MapContainer from './components/map/MapContainer.vue'
import FlightStats from './components/stats/FlightStats.vue'
import YearlyStats from './components/stats/YearlyStats.vue'
import LocationHeader from './components/header/LocationHeader.vue'
import RevenueStats from './components/header/RevenueStats.vue'
import FlightRoutes from './components/tables/FlightRoutes.vue'
import DroneStatus from './components/tables/DroneStatus.vue'
import FlightRecords from './components/tables/FlightRecords.vue'
</script>

<style lang="less" scoped>
// 样式已在全局CSS中定义
</style>
