<template>
  <div  class="event-add">
    <section>Новое событие</section>

    <label for="photo">Фото события</label>
    <input type="file" name="eventPhoto" id="photo" accept="image/png, image/jpeg, image/jpg" multiple
           @change="FileUpload">
    <img :src="new_event.photo" v-if="new_event.photo">
    <label for="title" >Название события</label>
    <input type="text" name="eventTitle" id="title" v-model="new_event.title">

    <label for="place">Место проведения</label>
    <input type="text" name="eventPlace" id="place" v-model="new_event.place">

    <label for="date">Дата проведения</label>
    <input type="date" name="eventDate" id="date" v-model="new_event.date">

    <label for="time">Время проведения</label>
    <input type="time" name="eventTime" id="time" v-model="new_event.time">

    <label for="short_detail">Краткое описание</label>
    <input type="text" name="eventShortDetail" id="short_detail" v-model="new_event.short_detail">

    <label for="age_rating">Возрастное ограничение</label>
    <input type="text" name="eventAgeRating" id="age_rating" v-model="new_event.age_rating">

    <label for="event_type">Тип события</label>
    <input type="text" name="eventType" id="event_type" v-model="new_event.event_type">

    <label for="event_tags">Тэги</label>
    <input type="text" name="eventTags" id="event_tags" v-model="new_event.event_tags">

    <label for="souls_count">Количество участников</label>
    <input type="number" name="eventSoulsCount" id="souls_count" v-model="new_event.souls_count">

    <label for="link">Ссылка</label>
    <input type="text" name="eventLink" id="link" v-model="new_event.link">

    <label for="coord_x">Координата X</label>
    <input type="number" name="eventCoordX" id="coord_x" v-model="new_event.coord_x">

    <label for="coord_y">Координата Y</label>
    <input type="number" name="eventCoordY" id="coord_y" v-model="new_event.coord_y">

    <button type="submit" @click="event_add">Добавить событие</button>
  </div>
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
  emits: ['create'],
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
      new_event: {
        photo: [],
        title: '',
        place: '',
        date: '',
        time: '',
        short_detail: '',
        age_rating: '',
        event_type: '',
        event_tags: '',
        souls_count: '',
        link: '',
        selectedEvent: null,
        coord_x: '',
        coord_y: '',
      },
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
      this.new_event.coord_x = this.markerPosition[0];
      this.new_event.coord_y = this.markerPosition[1];
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
    },
    FileUpload(event) {
      const file = event.target.files[0];
      let buff = URL.createObjectURL(file);
      this.new_event.photo.push(buff)
    },
    event_add() {
      this.new_event.id = Date.now();
      this.$emit('create', this.new_event);
      this.new_event = {
        photo: [],
        title: '',
        place: '',
        date: '',
        time: '',
        short_detail: '',
        age_rating: '',
        event_type: '',
        event_tags: '',
        souls_count: '',
        link: '',
        selectedEvent : null,
        coord_x: '',
        coord_y: '',
      }
      this.new_event.photo = '';
    },
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
.event-add {
  display: flex;
  align-items: flex-start;
  justify-content: center;
  flex-direction: column;
  border: 5px solid black;
  margin: 10px;
  border-radius: 5px;
  flex-wrap: nowrap;
  width: min-content;
  padding: 10px;
}
img {
  width: 250px;
  border: 5px solid black;
}
</style>