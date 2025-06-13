# 中空领航飞行运营数据驾驶舱

基于Vue3开发的eVTOL载人飞行器运营数据可视化大屏项目。

## 功能特性

- 高德地图实时展示eVTOL飞行器位置和飞行路线
- 飞行数据统计与实时监控
- 历年飞行数据统计图表
- eVTOL飞行器状态实时监控
- 飞行路线详细信息展示
- 飞行记录查询与管理

## 技术栈

- Vue 3 + Vite
- 高德地图 API
- ECharts 数据可视化
- Element Plus UI组件
- Less CSS预处理器

## 项目设置

### 安装依赖
```bash
npm install
```

### 开发服务器启动
```bash
npm run dev
```

### 构建生产版本
```bash
npm run build
```

## 项目结构

```
src/
├── assets/            # 静态资源
│   ├── images/        # 图片资源
│   ├── fonts/         # 字体资源
│   └── styles/        # 样式文件
├── components/        # 组件
│   ├── header/        # 头部组件
│   ├── map/           # 地图组件
│   ├── stats/         # 统计组件
│   └── tables/        # 表格组件
├── api/               # API接口
└── utils/             # 工具函数
```

## 高德地图API

本项目使用高德地图API，API Key已配置在地图组件中。

## 注意事项

1. 项目中的数据为模拟数据，实际使用时需替换为真实API数据
2. 高德地图API Key仅用于开发测试，部署生产环境时应申请正式Key
3. 数据大屏设计适合在大屏幕或宽屏显示器上观看

