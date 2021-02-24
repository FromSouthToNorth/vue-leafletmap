# vue-leaflet

# leaflet

## 1.安装 leaflet

### 1.1 yarn

~~~markdown
yarn add leaflet
~~~

### 1.2 npm

~~~markdown
npm install leaflet
~~~

### 1.3 cdn

~~~html
<!-- CSS style -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css"
integrity="sha512-xodZBNTC5n17Xt2atTPuE1HxjVMSvLVW9ocqUKLsCC5CXdbqCmblAshOMAS6/keqq/sMZMZ19scR4PsZChSR7A=="
crossorigin=""/>
<!-- JavaScript -->
<script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"
integrity="sha512-XQoYMqMTK8LvdxXYG3nZ448hOEQiglfqkJs1NOQV44cWnUrBc8PkAOcXy20w0vlaXaVUearIOBhiXZ5V3ynxwA=="
crossorigin=""></script>
~~~

## 2.引入 leaflet 文件及样式

~~~JavaScript
*/man.js

import Vue from 'vue'
import App from './App.vue'

import 'leaflet'
import 'leaflet/dist/leaflet.css'

new Vue({
  render: h => h(App),
}).$mount('#app')
~~~

## 3.在组件中使用 leafet

### 3.1 在组件钟放置地图的位置一个div元素id：

~~~vue
<div class="map-container" id="map-container"/>
~~~

### 3.2 确保容器具有定义的高度，避免无法显示。例如通过 class 属性在 css 中进行设置：

~~~css
.map-container {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}
~~~

### 3.3 在 data() 中声明挂载返回一个 map

~~~JavaScript
data() {
  return {
    map: null
  }
}
~~~

### 3.4 在 methods 中声明一个初始化地图方法

~~~JavaScript
initMap() {
    var map = L.map(
        'map-container',				// 绑定的元素 id
        /** options 配置 详情参见: https://leafletjs.com/reference-1.7.1.html*/
        {
            center: [51.505, -0.09], 	// 地图中心
            zomm: 13, 					// 缩放比例
            zoomControl: false,			// 禁用 + - 按钮
            doubleClickZomm: false,		// 禁用双击放大按钮
            attributionControl: false,	// 移除右下角 leaflet 标识链接
        }
    ) 
}
~~~

### 3.5 tileLayer 引入图层,可以引入多个图层

~~~JavaScript
// 地图url
let url = 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png'

// tileLayer 用于在地图上加载和显示图块图层
L.tileLayer(
    url,
    {
        attribution: '<a href="https://www.openstreetmap.org/">OSM</a>',
        maxZoom: 12,
    }
).addTo(map)
~~~

### 3.6 使用小图标显示位置

~~~JavaScript
// 用于渲染标记的图标实例
delete L.Icon.Default.prototype._getIconUrl
L.Icon.Default.mergeOptions({
    iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
    iconUrl: require("leaflet/dist/images/marker-icon.png"),
    shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
})

// 表示使用简单<div> 元素而不是图像的标记的轻量级图标。
var marker = L.marker([39.77284, 110.18982]).addTo(map)
~~~

## 4.使用 leafet markercluster

- [leafet.markercluster](https://github.com/Leaflet/Leaflet.markercluster#building-testing-and-linting-scripts)

### 4.1安装 leafet.markercluster

~~~javascript
yarn add leaflet.markercluster

npm install leaflet.markercluster
~~~

### 4.2引入 leafet.markercluster

~~~javascript
import 'leaflet.markercluster/dist/MarkerCluster.css'
import 'leaflet.markercluster/dist/MarkerCluster.Default.css'
import 'leaflet.markercluster'
~~~

### 4.3在initMap()方法中创建MarkerClusterGroup实例并使用

~~~javascript
var markers = new L.MarkerClusterGroup();

// map 数据经纬度聚合
for (let i = 0; i < layer.length; i++) {
    markers.addLayer(L.marker(layer[i], {
        icon: defaultIcon
    })
  )
}

map.addLayer(markers)
~~~

~~~javascript
module.exports = [
    [30.657395, 104.066379],
    [30.64942,  104.066035],
    [30.661898, 104.061658],
    [30.658724, 104.056766],
    [30.657468, 104.072816],
    [30.658871, 104.057023],
    [30.648756, 104.05771],
    [30.661677, 104.048612]
]
~~~

## 5.使用 MovingMarker

### 5.1 下载 MovingMarker 插件

- [MovingMarker .js](https://github.com/ewoken/Leaflet.MovingMarker)

### 5.2 导入 map 组件钟

~~~JavaScript
import '@/util/movingMarker'
~~~

### 5.3 简单的使用 movingMarker

~~~JavaScript
var movingMarker1 = L.Marker.movingMarker(
  latlngs, 	// 经纬度
  duration,	// 动画时间
  options,	// 配置
).addTo(map);
~~~

### 5.4 如果路径有多个点不但是两点相连，多个折线

~~~JavaScript
var paths = [
  [30.470539, 104.010379],
  [30.472744, 104.011098],
  [30.472467, 104.01467],
  [30.488976, 104.019209],
  [30.488509, 104.033001],
  [30.499677, 104.035629],
  [30.5121,   104.039792],
  [30.525317, 104.044727],
  [30.524568, 104.061904]
]

map.fitBounds(paths)

var movingMarker2 = L.Marker.movingMarker(paths,
  [4000, 4000, 4000, 4000, 2000, 2000, 2000, 4000],
  { autostart: true }).addTo(map)
L.polyline(paths, { color: 'blue', width: 1 }).addTo(map)
movingMarker2.on('end', () => {
  movingMarker2.bindPopup('<b>I is movingMarker2</b>', { closeOnClick: false }).openPopup()
})
~~~



## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
# vue-leafletmap

