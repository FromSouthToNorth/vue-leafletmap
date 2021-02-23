<template>
  <div class="map-collection" id="map-collection" />
</template>

<script>
import 'leaflet/dist/leaflet.css'
import 'leaflet'
import 'leaflet.markercluster/dist/MarkerCluster.css'
import 'leaflet.markercluster/dist/MarkerCluster.Default.css'
import 'leaflet.markercluster'

import layer from './layer'

export default {
  name: 'Map',
  data() {
    return {
      map: null,
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png'
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
        .setView([30.470853, 104.009746], 10)

      L.tileLayer(
        this.url,
        {
          attribution: '<a href="https://www.openstreetmap.org/">OSM</a>',
          maxZoom: 12,
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

      var markerWz = L.marker(
        [27.809299, 120.520408],
        {
          icon: iconWz
        }
      ).addTo(map).bindPopup('You am in WenZhou')

      var markerCd = L.marker(
        [30.470853, 104.009746],{
          icon: iconCd
        }
      ).addTo(map).bindPopup('I am in ChenDu')

      // 多边形
      // var polygon = L.polygon([
      //   layer
      // ],
      //   {
      //   color: '#03A9F4',
      //   weight: 1
      // }).addTo(map).bindPopup("hello！<br><b>I is polygon</b>")

      // 折线
      // var polyline = L.polyline([
      //   layer
      // ],
      //   {
      //     color: 'cyan',
      //     weight: 1
      // }).addTo(map).bindPopup("hello！<br><b>I is polyline</b>")

      var polyline = L.polyline([
        [30.470853, 104.009746],
        [27.809299, 120.520408]
      ],
        {
          color: 'green',
          weight: 1,
        }).addTo(map).bindPopup("<b>I am in ChenDu</b><br><b>You am in WenZhou</b>")
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

      // map.on('click', this.onMapClick())

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

      this.map = map
    },
    onMapClick(e) {
      alert("You clicked the map at")
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
