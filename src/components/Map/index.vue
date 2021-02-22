<template>
  <div class="map-collection" id="map-collection" />
</template>

<script>
import 'leaflet/dist/leaflet.css'
import 'leaflet'

const icon = require('./mapIcon/iconSetting')

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
    console.log(icon)
  },
  methods: {
    initMap() {
      delete L.Icon.Default.prototype._getIconUrl
      L.Icon.Default.mergeOptions({
        iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
        iconUrl: require("leaflet/dist/images/marker-icon.png"),
        shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
      })

      var map = L.map(
        'map-collection',
      {
        // 移除右下角 leaflet 标识链接
        attributionControl: false,
      })
        .setView([39.77287, 110.1897], 13)

      L.tileLayer(
        this.url,
      ).addTo(map)

      var marker = L.marker(
        [39.77287, 110.1897],
      ).addTo(map).bindPopup('I am a Popup');
      this.map = map
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
