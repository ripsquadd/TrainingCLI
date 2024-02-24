<template>
  <div  class="event-add">
    <section>Новое событие</section>
    <div>
      <label for="photo" class="input-file">Загрузить изабражения</label>
      <input type="file" name="eventPhoto" id="photo" accept="image/png, image/jpeg, image/jpg"
             @change="FileUpload" style="display: none;">
    </div>
    <div>
      <label for="title" >Название события</label>
      <input type="text" name="eventTitle" id="title" v-model="new_event.title">
    </div>
    <div>
      <label for="place">Место проведения</label>
      <input type="text" name="eventPlace" id="place" v-model="new_event.place">
    </div>
    <div>
      <label for="date">Дата проведения</label>
      <input type="date" name="eventDate" id="date" v-model="new_event.date">
    </div>
    <div>
      <label for="time">Время проведения</label>
      <input type="time" name="eventTime" id="time" v-model="new_event.time">
    </div>
    <div>
      <label for="short_detail">Краткое описание</label>
      <input type="text" name="eventShortDetail" id="short_detail" v-model="new_event.short_detail">
    </div>
    <div>
      <label for="age_rating">Возрастное ограничение</label>
      <input type="text" name="eventAgeRating" id="age_rating" v-model="new_event.age_rating">
    </div>
    <div>
      <label for="event_type">Тип события</label>
      <input type="text" name="eventType" id="event_type" v-model="new_event.event_type">
    </div>
    <div>
      <label for="event_tags">Тэги</label>
      <input type="text" name="eventTags" id="event_tags" v-model="new_event.event_tags">
    </div>
    <div>
      <label for="souls_count">Количество участников</label>
      <input type="number" name="eventSoulsCount" id="souls_count" v-model="new_event.souls_count">
    </div>
    <div>
      <label for="link">Ссылка</label>
      <input type="text" name="eventLink" id="link" v-model="new_event.link">
    </div>
    <button type="submit" @click="event_add">Добавить событие</button>
  </div>
  <event-map :events="events"
             :selectedEvent="selectedEvent"
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
      imageUrl: '',
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
  align-items: stretch;
  flex-direction: column;
  margin: 10px;
  border-radius: 5px;
  flex-wrap: nowrap;
  width: -moz-min-content;
  width: min-content;
  padding: 10px;
  background: #1B1212;
  font-size: 14pt;
  justify-content: space-between;
}
.event-add > div {
  display: flex;
  align-items: center;
  align-content: center;
  flex-wrap: nowrap;
}
.event-add > section {
  color: #EADDCA ;
  text-shadow: #FF5733 1px 0 15px;
  text-align: center;
  font-size: 20pt;
}
.event-add > div > label {
  color: #EADDCA ;
  text-shadow: #F88379 1px 0 15px;
  padding: 10px;
  width: max-content;
}
.event-add > div > input {
  color: #EADDCA ;
  background: black;
  border: none;
  border-radius: 5px;
  height: 20px;
}
.event-add > button {
  margin: 10px;
  border: none;
  background: #1B1212;
  color: #FFA500 ;
  text-shadow: #FF5733 1px 0 15px;
  font-size: 14pt;
}
.event-add > div > button:hover  {
  color: #FF69B4;
  text-shadow: #FF10F0 1px 0 20px;
}
.input-file {
  color: #EADDCA ;
  background: black;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
.input-file:hover {
  color: #FF69B4;
  text-shadow: #FF10F0 1px 0 20px;
}
img {
  width: 250px;
  height: 250px;
  background: #1B1212;
}
#map {
  z-index: 0;
}
</style>