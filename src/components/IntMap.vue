<template>
  <l-map id="map" style="height:600px; width:800px" ref="map"
         :zoom="zoom" :center="center"
         @click="addMarker" @moveend="updateData">
    <l-marker :lat-lng="markerPosition"
              @move="updateMarkerLatLng"></l-marker>
    <l-tile-layer layer-type="base"
                  name="OpenStreetMap"
                  :url="url"
                  :attribution="attribution">
    </l-tile-layer>
  </l-map>
</template>

<script>
import { LMap, LTileLayer, LMarker } from '@vue-leaflet/vue-leaflet';
import 'leaflet/dist/leaflet.css';
import L from 'leaflet'
import icon from 'leaflet/dist/images/marker-icon.png';
import iconShadow from 'leaflet/dist/images/marker-shadow.png';

export default {
  name: "IntMap",
  components: {
    LMap,
    LTileLayer,
    LMarker
  },
  data() {
    return {
      zoom: 11,
      center: [56.4853, 84.9885],
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution: 'Spotlight',
      x : 0,
      y : 0,
      markerPosition: [0, 0],
      viewPoint : {
        viewPointHeight: 0,
        viewPointWidth: 0,
      },
      leftPoint : [0, 0],
    };
  },
  methods : {
    addMarker(e) {
      this.x = e.latlng.lat
      this.y = e.latlng.lng
      this.markerPosition = [this.x, this.y];
    },
    updateMarkerLatLng(event) {
      this.markerPosition = [event.target._latlng.lat, event.target._latlng.lng];
      console.log("Новые координаты маркера: ",this.markerPosition)
    },
    updateData(event) {
      this.center = event.target.getCenter();
      console.log("Новая центральная точка карты: ", this.center);
      this.leftPoint = event.target.getBounds().getNorthWest();
      console.log("Новая верхняя левая точка карты: ", this.leftPoint);
      this.viewPoint.viewPointHeight = document.getElementById('map').clientHeight;
      this.viewPoint.viewPointWidth = document.getElementById('map').clientWidth;
      console.log("Новые данные о Вьюпинте: ",this.viewPoint);
    }
  },
  mounted() {

  }
}
</script>

<style scoped>
l-marker {
  height: 10px;
  width: 10px;
  background: black;
}
</style>