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
    [30.661677, 104.048612],
    [30.660939, 104.074533],
    [30.665221, 104.050414],
    [30.66655,  104.072473],
    [30.65769,  104.08509],
    [30.651414, 104.076421],
    [30.652891, 104.05213],
    [30.636793, 104.076249],
    [30.645876, 104.049728],
    [30.633839, 104.052217],
    [30.648386, 104.057624],
    [30.647279, 104.075305],
    [30.653038, 104.079854],
    [30.64307,  104.086463],
    [30.631402, 104.08320],
    [30.597423, 104.084746],
    [30.616482, 104.042003],
    [30.616039, 104.043376],
    [30.611607, 104.061916],
    [30.60614,  104.068439],
    [30.615448, 104.010932],
    [30.661382, 104.062345],
    [30.670758, 104.076593],
    [30.675039, 104.072644],
    [30.673637, 104.047925],
    [30.664335, 104.07685],
    [30.670536, 104.089982],
    [30.660939, 104.054277],
    [30.674006, 104.047324],
    [30.659536, 104.040887],
    [30.65134,  104.041402],
    [30.684709, 104.067838],
    [30.659914, 104.067623],
    [30.660699, 104.067065],
    [30.659471, 104.067087],
    [30.659951, 104.064426],
    [30.658927, 104.06493],
    [30.65949,  104.063857],
    [30.659499, 104.063933],
    [30.659277, 104.062785],
    [30.658567, 104.06492],
    [30.658742, 104.067881],
    [30.657939, 104.06389],
    [30.657782, 104.064426],
    [30.658078, 104.065649],
    [30.657921, 104.065864],
    [30.657579, 104.066325],
    [30.524548, 104.061963],
    [30.525694, 104.069859],
    [30.525953, 104.067285],
    [30.527579, 104.055161],
    [30.522385, 104.056856],
    [30.488387, 104.033274],
    [30.470486, 104.01216],
    [30.470782, 104.027008],
    [30.471115, 104.012074],
    [30.470662, 104.01305],
    [30.471355, 104.014027],
    [30.472701, 104.013093],
    [30.472645, 104.012085],
    [30.500816, 104.040895],
    [30.501972, 104.040627],
    [30.502545, 104.042494],
    [30.501879, 104.042247],
    [30.502277, 104.043878],
    [30.502462, 104.043722],
    [30.501847, 104.044006],
    [30.50163,  104.044098],
    [30.501829, 104.043218],
    [30.502295, 104.040756],
    [30.540554, 104.059429],
    [30.530943, 104.055309],
    [30.527912, 104.053335],
    [30.530296, 104.056511],
    [30.530241, 104.056554],
    [30.529908, 104.054311],
    [30.529243, 104.054869],
    [30.528864, 104.054848],
    [30.527912, 104.055325],
    [30.527875, 104.055529],
    [30.527792, 104.054848],
    [30.527718, 104.055164],
    [30.527773, 104.055443],
    [30.527607, 104.055685],
    [30.527736, 104.055256],
    [30.527404, 104.055315],
    [30.528032, 104.054805],
    [30.527353, 104.055062],
    [30.527339, 104.0554],
    [30.527487, 104.055577],
    [30.527279, 104.055057],
    [30.527376, 104.055159],
    [30.5273,   104.055682],
    [30.527376, 104.055407],
    [30.58636,  104.06725],
    [30.584624, 104.069815],
    [30.585192, 104.07019],
    [30.584719, 104.069225],
    [30.569928, 103.955571],
    [30.57039,  103.956816],
    [30.569263, 103.957309],
    [30.567138, 103.953404],
    [30.567637, 103.954992],
    [30.571794, 103.956462],
    [30.572976, 103.956161],
    [30.628938, 104.141019],
    [30.624729, 104.14235],
    [30.630046, 104.138144],
    [30.627794, 104.144066],
    [30.657443, 104.102867],
    [30.685791, 104.064415],
    [30.672799, 104.069908],
    [30.643856, 104.056176],
    [30.685745, 103.978724],
    [30.526858, 104.066497],
    [30.526706, 104.066749],
    [30.526456, 104.066143],
    [30.526456, 104.066706],
    [30.526632, 104.067409],
    [30.526447, 104.066947],
    [30.526433, 104.067527],
    [30.526733, 104.068471],
    [30.526405, 104.068508],
    [30.526216, 104.068374],
    [30.526077, 104.067693],
    [30.52598,  104.06861],
    [30.525855, 104.068009],
    [30.526012, 104.066663],
    [30.526202, 104.069018]
]
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

