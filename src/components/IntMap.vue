<template>
  <div  class="event-add" v-if="event_form_on">
    <div class="close-btn">
      <button @click="closeEventAddFormEmit">X</button>
    </div>
    <section>Новое событие</section>
    <div>
      <p>Нажмите на карту чтобы выбрать координаты события</p>
    </div>
    <div>
      <label for="photo" class="input-file">Загрузить изображения</label>
      <input type="file" name="eventPhoto" id="photo" accept="image/png, image/jpeg, image/jpg"
             @change="FileUploadEvent" style="display: none;">
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

  <div  class="event-add" v-if="place_form_on">
    <div class="close-btn">
      <button @click="closePlaceAddFormEmit">X</button>
    </div>
    <section>Новое место</section>
    <div>
      <p>Нажмите на карту чтобы выбрать координаты места</p>
    </div>
    <div>
      <label for="photo" class="input-file">Загрузить изображения</label>
      <input type="file" name="eventPhoto" id="photo" accept="image/png, image/jpeg, image/jpg"
             @change="FileUploadPlace" style="display: none;">
    </div>
    <div>
      <label for="title" >Название места</label>
      <input type="text" name="eventTitle" id="title" v-model="new_place.name">
    </div>
    <div>
      <label for="place">Описание места</label>
      <input type="text" name="eventPlace" id="place" v-model="new_place.description">
    </div>
    <div>
      <label for="placeAddress">Адрес</label>
      <input type="text" name="placeAddress" id="place_address" v-model="new_place.address">
    </div>
    <div>
      <label for="placeType">Тип места</label>
      <input type="text" name="placeType" id="place_type" v-model="new_place.place_type">
    </div>
    <button type="submit" @click="place_add">Добавить место</button>
  </div>

  <event-map :events="events" v-if="showEvent"
             :selectedEvent="selectedEvent"
             :showEvent="showEvent"
             @event_transfer_to_death="event_down"
             @event_close="eventClose"
             @event_taking_part="eventTakingPart"
             @event_un_parting="eventUnParting"
             @save_event_edit="saveNewEventData"
  />

  <place-window :places="places" v-if="showPlace"
             :selectedPlace="selectedPlace"
             :showPlace="showPlace"
             @place_transfer_to_death="place_down"
             @place_close="placeClose"
             @save_place_edit="saveNewPlaceData"
  />

  <l-map id="map" ref="map" style="width:97vw;height:600px;"
         :zoom="zoom" :center="center"
         @click="updateMarkerLatLng" @moveend="updateData">
    <l-marker
        v-for="(event_marker, id) in events"
        :lat-lng="event_marker.coords"
        @click="showEventDescription(event_marker.id)"
    >
      <l-icon :icon-url="eventMarkerIconUrl" :icon-size="[120, 120]"></l-icon>
    </l-marker>
    <l-marker
        v-for="(place_marker, id) in places"
        :lat-lng="place_marker.coords"
        @click="showPlaceDescription(place_marker.id)"
    >
      <l-icon :icon-url="placeMarkerIconUrl" :icon-size="[100, 100]"></l-icon>
    </l-marker>
    <l-tile-layer layer-type="base"
                  name="OpenStreetMap"
                  :url="url"
                  :attribution="attribution">
    </l-tile-layer>
  </l-map>
</template>

<script>
import { LMap, LTileLayer, LMarker, LIcon } from '@vue-leaflet/vue-leaflet';
import EventMap from "@/components/EventMap";
import PlaceWindow from "@/components/PlaceWindow";
import 'leaflet/dist/leaflet.css';

export default {
  name: "IntMap",
  emits: ['event_create', 'event_delete', 'event_add_form_close',
    'take_a_part', 'un_part', 'save_edit_to_app', 'place_create',
    'event_delete', 'place_add_form_close', 'save_place_to_app',
    'place_delete'],
  props: {
    events: {
      type: Array,
      required: true
    },
    event_form_on: {
      type: Boolean
    },
    places: {
      type: Array,
      required: true
    },
    place_form_on : {
      type: Boolean
    },
    organizations: {
      type: Array,
      required: true
    },
    organization_form_on : {
      type: Boolean
    },
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
    EventMap,
    PlaceWindow,
    LIcon
  },
  data() {
    return {
      showEvent: true,
      showPlace: true,
      zoom: 11,
      center: [56.4853, 84.9885],
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution: 'Spotlight',
      eventMarkerIconUrl: require("./components_assets/event.png"),
      placeMarkerIconUrl: require("./components_assets/place.png"),
      organizationMarkerIconUrl: require("./components_assets/organization.png"),
      x : 0,
      y : 0,
      markerPosition: [0, 0],
      viewPoint : {
        viewPointHeight: 0,
        viewPointWidth: 0,
      },
      leftPoint : [0, 0],
      selectedEvent: null,
      selectedPlace: null,
      imageUrl: '',
      new_place: {
        id: '',
        name: '',
        description: '',
        coord_x: '',
        coord_y: '',
        coords: '',
        photo: '',
        address: '',
        place_type: '',
        selectedEvent: null,
      },
      new_event: {
        id: '',
        photo: '',
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
    saveNewEventData (edit_event, buff_id) {
      this.$emit('save_edit_to_app', edit_event, buff_id);
    },
    saveNewPlaceData (edit_place, buff_id) {
      this.$emit('save_place_to_app', edit_place, buff_id);
    },
    eventTakingPart(id) {
      this.$emit('take_a_part', id);
    },
    eventUnParting(id) {
      this.$emit('un_part', id);
    },
    updateMarkerLatLng(event) {
      this.markerPosition = [event.latlng.lat, event.latlng.lng];
      this.new_event.coord_x = this.markerPosition[0];
      this.new_event.coord_y = this.markerPosition[1];
      this.new_event.coords = this.markerPosition;

      this.new_place.coord_x = this.markerPosition[0];
      this.new_place.coord_y = this.markerPosition[1];
      this.new_place.coords = this.markerPosition;

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
    FileUploadEvent(event) {
      const file = event.target.files[0];
      this.new_event.photo = URL.createObjectURL(file);
    },
    FileUploadPlace(event) {
      const file = event.target.files[0];
      this.new_place.photo = URL.createObjectURL(file);
    },
    event_add() {
      if (this.new_event.coord_x && this.new_event.coord_y) {
        this.new_event.id = Date.now();
        this.$emit('event_create', this.new_event);
        this.new_event = {
          photo: '',
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
        this.$emit('event_add_form_close');
      }
    },
    place_add() {
      if (this.new_place.coord_x && this.new_place.coord_y) {
        this.new_place.id = Date.now();
        this.$emit('place_create', this.new_place);
        this.new_place = {
          name: '',
          description: '',
          coord_x: '',
          coord_y: '',
          coords: '',
          photo: [],
          selectedPlace : null,
          address: '',
          place_type: ''
        }
        this.new_place.photo = '';
        this.$emit('place_add_form_close');
      }
    },
    event_down(id) {
      this.$emit('event_delete', id);
    },
    place_down(id) {
      this.$emit('place_delete', id);
    },
    showEventDescription(id) {
      this.showPlace = false;
      this.selectedEvent = id;
      this.showEvent = true;
      this.event_item(this.selectedEvent);
    },
    showPlaceDescription(id) {
      this.showEvent = false;
      this.selectedPlace = id;
      this.showPlace = true;
      this.place_item(this.selectedPlace);
    },
    eventClose() {
      this.showEvent = false;
    },
    placeClose() {
      this.showPlace = false;
    },
    event_item(selectedId) {
      console.log(selectedId)
      return this.events.find(item => item.id === selectedId);
    },
    place_item(selectedId) {
      console.log(selectedId)
      return this.places.find(item => item.id === selectedId);
    },
    closeEventAddFormEmit() {
      this.$emit('event_add_form_close');
    },
    closePlaceAddFormEmit() {
      this.$emit('place_add_form_close');
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
.close-btn {
  display: flex;
  flex-direction: row-reverse;
}
.close-btn > button {
  display: flex;
  flex-direction: row-reverse;
  margin: 5px;
  border: none;
  background: #1B1212;
  color: #FFA500 ;
  text-shadow: #FF5733 1px 0 15px;
  font-size: 14pt;
}
.close-btn > button:hover {
  color: #FF69B4;
  text-shadow: #FF10F0 1px 0 20px;
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
  z-index: 2;
  position: absolute;
  left: 5%;
  top: 6%;
}
.event-add > div {
  display: flex;
  align-items: center;
  align-content: center;
  flex-wrap: nowrap;
}
.event-add > section {
  color: #C4A484; ;
  text-align: center;
  font-size: 20pt;
}
.event-add > div > label {
  color: #C4A484;
  padding: 10px;
  width: max-content;
}
.event-add > div > p {
  color: #C4A484;
  padding: 10px;
}
.event-add > div > input {
  color: #ffffff;
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
.event-add > button:hover  {
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
  margin:10px;
  border-radius:5px;
}
</style>