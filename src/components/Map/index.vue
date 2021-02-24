<template>
  <div class="map-collection" id="map-collection" />
</template>

<script>
import 'leaflet/dist/leaflet.css'
import 'leaflet'
import 'leaflet.markercluster/dist/MarkerCluster.css'
import 'leaflet.markercluster/dist/MarkerCluster.Default.css'
import 'leaflet.markercluster'
import '@/util/movingMarker'
import 'leaflet-ant-path'

import layer from './layer'

export default {
  name: 'Map',
  data() {
    return {
      map: null,
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      loops: 0
    }
  },
  mounted() {
    this.initMap()
  },
  created() {
  },
  methods: {
    initMap() {
      var map = L.map(
        'map-collection',
      {
        center: [30.470853, 104.009746],
        // 移除右下角 leaflet 标识链接
        attributionControl: false,
      })
        .setView([30.470853, 104.009746], 7)

      L.tileLayer(
        this.url,
        {
          attribution: '<a href="https://www.openstreetmap.org/">OSM</a>',
          maxZoom: 19,
        }
      ).addTo(map)

      var iconCd = L.icon({
        iconUrl:      require('@/assets/images/icon/leaf-green.png'),
        shadowUrl:    require('@/assets/images/icon/leaf-shadow.png'),

        iconSize:     [38, 95],   // size of the icon
        shadowSize:   [50, 64],   // size of the shadow
        iconAnchor:   [22, 94],   // 对应于标记位置的图标点
        shadowAnchor: [4, 62],    // the same for the shadow
        popupAnchor:  [-3, -76]   // 相对于图标锚，弹出框应该从哪个点打开
      })

      var iconWz = L.icon({
        iconUrl:      require('@/assets/images/icon/leaf-red.png'),
        shadowUrl:    require('@/assets/images/icon/leaf-shadow.png'),

        iconSize:     [38, 95],   // size of the icon
        shadowSize:   [50, 64],   // size of the shadow
        iconAnchor:   [22, 94],   // 对应于标记位置的图标点
        shadowAnchor: [4, 62],    // the same for the shadow
        popupAnchor:  [-3, -76]   // 相对于图标锚，弹出框应该从哪个点打开
      })

      var defaultIcon = L.icon({
        iconUrl: require('leaflet/dist/images/marker-icon.png'),
        iconShadow : require('leaflet/dist/images/marker-icon.png')
      })

      // var markerWz = L.marker(
      //   [27.809299, 120.520408],
      //   {
      //     icon: iconWz
      //   }
      // ).addTo(map).bindPopup('You am in WenZhou')

      var markerCd = L.marker(
        [30.470853, 104.009746],{
          icon: iconCd
        }
      ).addTo(map).bindPopup('I am in ChengDu')

      var movingMarkerIcon1 = L.icon(
        {
          iconUrl:      require('@/assets/images/amazed.jpg'),
          iconSize:     [46, 28],   // size of the icon
        }
      )

      var movingMarkerIcon2 = L.icon(
        {
          iconUrl:      require('@/assets/images/giggle.jpg'),
          iconSize:     [40, 32],   // size of the icon
        }
      )

      var movingMarkerIcon3 = L.icon(
        {
          iconUrl:      require('@/assets/images/speechless.jpg'),
          iconSize:     [46, 32],   // size of the icon
        }
      )

      //************ 轨迹运行 **********
      //==================================================
      var movingMarker1 = L.Marker.movingMarker(
        [[30.470853, 104.009746], [30.524631, 104.061842]],
        [20000],
        {
          icon: movingMarkerIcon1
        }).addTo(map);

      movingMarker1.once('click', (e) => {
        // 启动路径动画
        movingMarker1.start()
        // 关闭窗口
        movingMarker1.closePopup()
        // 解除绑定窗口
        movingMarker1.unbindPopup()
        movingMarker1.on('click', (e) => {
          // 如果正在启动结束
          if (movingMarker1.isRunning()) {
            movingMarker1.pause()
          } else {
            // 如果停止重新启动
            movingMarker1.start()
          }
        })
        setTimeout(() => {
          movingMarker1.bindPopup('<b>点击我暂停</b>').openPopup()
        }, 2000)
      })

      movingMarker1.bindPopup('<b>点击我开始</b>',
        {
          closeOnClick: false,
        })
      movingMarker1.openPopup()

      //==================================================
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

      var paths2 = [
        [30.57507,  104.028729],
        [30.49937,  103.966759],
        [30.416219, 103.908537],
        [30.339923, 104.119237],
        [30.567532, 104.110561],
      ]

      map.fitBounds(paths)
      var movingMarker2 = L.Marker.movingMarker(paths,
        [4000, 4000, 4000, 4000, 2000, 2000, 2000, 4000],
        { icon: movingMarkerIcon2, autostart: true }).addTo(map)
      L.polyline(paths, { color: 'blue', width: 1 }).addTo(map)

      movingMarker2.on('end', () => {
        movingMarker2.bindPopup('<b>I is movingMarker2</b>', { closeOnClick: false }).openPopup()
      })

      //==================================================
      var movingMarker3 = L.Marker.movingMarker(paths2,
      [10000, 10000, 10000, 10000, 10000],
        { icon: movingMarkerIcon3, autostart: true, loop: true }).addTo(map)

      movingMarker3.bindPopup('', { closeOnClick: false })

      //==================================================
      var movingMarker4 = L.Marker.movingMarker(
        [[30.486644, 104.051581]],
      [], { icon: movingMarkerIcon3 }).addTo(map)

      movingMarker3.on('loop', (e) => {
        this.loops++
        if (e.elapsedTime < 50) {
          movingMarker3.getPopup().setContent('<b>Loop: ' + this.loops + ' </b>')
          movingMarker3.openPopup()
          setTimeout(() => {
            movingMarker3.closePopup()

            if (!movingMarker3.isEnded()) {
              movingMarker3.openPopup()
            } else {
              if (movingMarker4.getLatLng().equals([30.486644, 104.051581])) {
                movingMarker4.bindPopup('点击地图来移动我')
                movingMarker4.openPopup()
              }
            }
          }, 2000)
        }
      })

      movingMarker3.on('click', (e) => {
        if (movingMarker3.isRunning()) {
          movingMarker3.pause()
        } else {
          movingMarker3.start()
        }
      })

      map.on('click', (e) => {
        movingMarker4.moveTo(e.latlng, 2000)
      })

      //==================================================
      var movingMarker5 = L.Marker.movingMarker(
        paths2,
        10000,
        {icon: movingMarkerIcon3, autostart: true}
      ).addTo(map)

      movingMarker5.addStation(1, 2000)
      movingMarker5.addStation(2, 2000)
      movingMarker5.addStation(3, 2000)
      movingMarker5.addStation(4, 2000)
      movingMarker5.addStation(5, 2000)

      // 多边形
      var polygon = L.polygon([
        [30.573916, 104.028893],
        [30.499269, 103.968025],
        [30.418412, 103.910251],
        [30.383909, 103.965419],
        [30.364906, 104.03715],
        [30.339859, 104.119248],
        [30.415962, 104.12927],
        [30.443066, 104.121494],
        [30.442117, 104.121362],
        [30.568003, 104.111904]
      ],
        {
        color: 'green',
        weight: 1.5
      }).addTo(map)

      // 折线
      var polyline = L.polyline([
          [30.470853, 104.009746],
          [30.524631, 104.061842]
      ],
        {
          color: 'red',
          weight: 2
      }).addTo(map).bindPopup("hello！<br><b>I is polyline</b>")

      // var polyline = L.polyline([
      //   [30.470853, 104.009746],
      //   [27.809299, 120.520408]
      // ],
      //   {
      //     color: 'green',
      //     weight: 1,
      //   }).addTo(map).bindPopup("<b>I am in ChengDu</b><br><b>You am in WenZhou</b>")

      //
      // var greenCircle = L.circle(
      //   [30.470853, 104.009746],
      //   {
      //     color: '#B9F6CA',
      //     fillColor: '#81C784',
      //     fillOpacity: 0.5,
      //     radius: 10000
      // }).addTo(map)
      //
      // var pinkCircle = L.circle(
      //   [27.809299, 120.520408],
      //   {
      //     color: '#FF80AB',
      //     fillColor: '#F48FB1',
      //     fillOpacity: 0.5,
      //     radius: 10000
      //   }).addTo(map)

      var markers = new L.MarkerClusterGroup();

      // map 数据经纬度聚合
      for (let i = 0; i < layer.length; i++) {
        markers.addLayer(L.marker(layer[i],
          {
            icon: defaultIcon
          })
        )
      }

      map.addLayer(markers)

      // 当用户单击（或点击）图层时触发
      // map.on('click', (e) => {
      //   console.log(e)
      // })

      // 鼠标进入图层
      // map.on('mouseover', (e) => {
      //   console.log(e)
      // })

      const locations = []

      for (let  i = 0; i < 1000; i++) {
        locations.push(
          [this.rand(30.470853), this.rand(104.009746)]
        )
      }

      var path3 = [
        [30.628852, 104.141286],
        [30.668538, 104.156754],
        [30.685489, 104.2055],
        [30.717075, 104.321972],
        [30.700988, 104.344803],
        [30.709696, 104.359566],
        [30.506845, 105.572182],
        [30.80219,  106.091286],
        [30.456689, 106.631749]
      ]

      let antPolyline = new L.Polyline.AntPath(path3, {
        color: "#607D8B",
      })

      antPolyline.addTo(map)

      this.map = map
    },
    rand(n) {
      const max = n + 0.01
      const min = n + 0.01
      return Math.random() * (max - min) + min
    }
  }
}
</script>

<style lang="scss" scoped>
.map-collection {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}
</style>
