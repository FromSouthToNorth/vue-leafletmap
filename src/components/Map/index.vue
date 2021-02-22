<template>
  <div class="map-collection" id="map-collection" />
</template>

<script>
import 'leaflet/dist/leaflet.css'
import 'leaflet'

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
        // 移除右下角 leaflet 标识链接
        attributionControl: false,
      })
        .setView([30.471258, 104.011849], 13)

      L.tileLayer(
        this.url,
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

      var markerCd = L.marker(
        [30.471258, 104.011849],{
          icon: iconCd
        }
      ).addTo(map).bindPopup('I am in ChenDu')

      var markerWz = L.marker(
        [27.809299, 120.520408],
        {
          icon: iconWz
        }
      ).addTo(map).bindPopup('You am in WenZhou').openPopup()

      var polygon = L.polygon([
        [30.471258, 104.011849],
        [27.809299, 120.520408]
      ], {
        color: '#F8BBD0',
      }).addTo(map).bindPopup()

      var greenCircle = L.circle(
        [30.471258, 104.011849],
        {
          color: '#B9F6CA',
          fillColor: '#81C784',
          fillOpacity: 0.5,
          radius: 50000
      }).addTo(map)

      var pinkCircle = L.circle(
        [27.809299, 120.520408],
        {
          color: '#FF80AB',
          fillColor: '#F48FB1',
          fillOpacity: 0.5,
          radius: 50000
        }).addTo(map)

      // map.on('click', this.onMapClick())

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
  margin-right: auto;
  margin-left: auto;
  padding: 20px;
  width: 800px;
  height: 600px;
}
</style>
