<template>
  <div class="location-header">
    <div class="location-item" :class="{ active: currentProvince === '云南省' }">
      云南省 <i class="el-icon-caret-bottom"></i>
    </div>
    <div class="location-item" :class="{ active: currentCity === '大理市' }">
      大理市 <i class="el-icon-caret-bottom"></i>
    </div>
    <div class="location-item active">
      大理古城-苍山洱海景区
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const currentProvince = ref('云南省')
const currentCity = ref('大理市')
</script>

<style lang="less" scoped>
.location-header {
  display: flex;
  align-items: center;
  height: 40px;
  margin-bottom: 10px;
  background-color: rgba(9, 31, 60, 0.6);
  border: 1px solid var(--border-color);
  border-radius: 4px;
  padding: 0 15px;
}

.location-item {
  display: flex;
  align-items: center;
  color: rgba(255, 255, 255, 0.7);
  font-size: 14px;
  cursor: pointer;
  padding: 0 15px;
  position: relative;
  
  &:not(:last-child)::after {
    content: '';
    position: absolute;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
    width: 1px;
    height: 14px;
    background-color: var(--border-color);
  }
  
  &:hover {
    color: var(--text-color);
  }
  
  &.active {
    color: var(--text-color);
    font-weight: bold;
  }
  
  i {
    margin-left: 5px;
    font-size: 12px;
  }
}
</style> 