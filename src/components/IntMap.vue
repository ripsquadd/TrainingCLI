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
  <event-map :events="events"
             v-if="item.id === selectedEvent"
             @event_transfer_to_death="event_down"/>
  <l-map id="map" style="height:600px; width:800px" ref="map"
         :zoom="zoom" :center="center"
         @click="updateMarkerLatLng" @moveend="updateData">
    <l-marker
        v-for="(event_marker, id) in events"
        :lat-lng="event_marker.coords"
        @click="showDescription(event_marker.id)"
    ></l-marker>
    <l-tile-layer layer-type="base"
                  name="OpenStreetMap"
                  :url="url"
                  :attribution="attribution">
    </l-tile-layer>
  </l-map>
</template>

<script>
import { LMap, LTileLayer, LMarker } from '@vue-leaflet/vue-leaflet';
import EventMap from "@/components/EventMap";
import 'leaflet/dist/leaflet.css';
import L from 'leaflet'
import icon from 'leaflet/dist/images/marker-icon.png';
import iconShadow from 'leaflet/dist/images/marker-shadow.png';

export default {
  name: "IntMap",
  emits: ['create', 'event_delete'],
  props: {
    events: {
      type: Array,
      required: true
    },
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
    EventMap
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
      selectedEvent: null,
      new_event: {
        id: '',
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
        coords: '',
      },
    };
  },
  methods : {
    updateMarkerLatLng(event) {
      this.markerPosition = [event.latlng.lat, event.latlng.lng];
      this.new_event.coord_x = this.markerPosition[0];
      this.new_event.coord_y = this.markerPosition[1];
      this.new_event.coords = this.markerPosition;
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
        coords: '',
      }
      this.new_event.photo = '';
    },
    event_down(id) {
      this.$emit('event_delete', id);
    },
    showDescription(id) {
      this.selectedEvent = id;
      console.log(this.selectedEvent);
      this.item(this.selectedEvent);
    },
    item(selectedId) {
      console.log(selectedId)
      return this.events.find(item => item.id === selectedId);
    },
  },
  mounted() {

  },
  computed: {

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