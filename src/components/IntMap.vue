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
      <input type="file" id="photo" multiple @change="handleFileUploadEvent" style="display: none">
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
      <input type="file" id="photo" multiple @change="handleFileUploadPlace" style="display: none">
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

  <div  class="event-add" v-if="organization_form_on">
    <div class="close-btn">
      <button @click="closeOrganizationAddFormEmit">X</button>
    </div>
    <section>Новая организация</section>
    <div>
      <p>Нажмите на карту чтобы выбрать координаты организации</p>
    </div>
    <div>
      <label for="photo" class="input-file">Загрузить изображения</label>
      <input type="file" id="photo" multiple @change="handleFileUploadOrganization" style="display: none">
    </div>
    <div>
      <label for="title" >Название организации</label>
      <input type="text" name="eventTitle" id="title" v-model="new_organization.name">
    </div>
    <div>
      <label for="place">Описание организации</label>
      <input type="text" name="eventPlace" id="place" v-model="new_organization.description">
    </div>
    <div>
      <label for="placeAddress">Адрес</label>
      <input type="text" name="placeAddress" id="place_address" v-model="new_organization.address">
    </div>
    <button type="submit" @click="organization_add">Добавить организацию</button>
  </div>

  <event-map :events="events" v-if="showEvent && searchResultsOn === false"
             :selectedEvent="selectedEvent"
             :showEvent="showEvent"
             @event_transfer_to_death="event_down"
             @event_close="eventClose"
             @event_taking_part="eventTakingPart"
             @event_un_parting="eventUnParting"
             @save_event_edit="saveNewEventData"
  />
  <event-map :events="searchResultsEvents" v-if="showEvent && searchResultsOn === true"
             :selectedEvent="selectedEvent"
             :showEvent="showEvent"
             @event_transfer_to_death="event_down"
             @event_close="eventClose"
             @event_taking_part="eventTakingPart"
             @event_un_parting="eventUnParting"
             @save_event_edit="saveNewEventData"
  />

  <place-window
      :places="places" v-if="showPlace && searchResultsOn === false"
      :selectedPlace="selectedPlace"
      :showPlace="showPlace"
      @place_transfer_to_death="place_down"
      @place_close="placeClose"
      @save_place_edit="saveNewPlaceData"
  />
  <place-window
      :places="searchResultsPlaces" v-if="showPlace && searchResultsOn === true"
      :selectedPlace="selectedPlace"
      :showPlace="showPlace"
      @place_transfer_to_death="place_down"
      @place_close="placeClose"
      @save_place_edit="saveNewPlaceData"
  />

  <organization-window
      :organizations="organizations" v-if="showOrganization && searchResultsOn === false"
      :selectedOrganization="selectedOrganization"
      :showOrganization="showOrganization"
      @organization_transfer_to_death="organization_down"
      @organization_close="organizationClose"
      @save_organization_edit="saveNewOrganizationData"
  />
  <organization-window
      :organizations="searchResultsOrganizations" v-if="showOrganization && searchResultsOn === true"
      :selectedOrganization="selectedOrganization"
      :showOrganization="showOrganization"
      @organization_transfer_to_death="organization_down"
      @organization_close="organizationClose"
      @save_organization_edit="saveNewOrganizationData"
  />

  <l-map id="map" ref="map" style="width:97vw;height:600px;"
         :zoom="zoom" :center="center"
         @click="updateMarkerLatLng" @moveend="updateData">

    <l-marker v-if="sort_events && searchResultsOn === false"
        v-for="(event_marker, id) in events"
        :lat-lng="event_marker.coords"
        @click="showEventDescription(event_marker.id)"
    >
      <l-icon :icon-url="eventMarkerIconUrl" :icon-size="[120, 120]"></l-icon>
    </l-marker>
    <l-marker v-if="sort_events && searchResultsOn === true"
              v-for="(event_marker, id) in searchResultsEvents"
              :lat-lng="event_marker.coords"
              @click="showEventDescription(event_marker.id)"
    >
      <l-icon :icon-url="eventMarkerIconUrl" :icon-size="[120, 120]"></l-icon>
    </l-marker>


    <l-marker v-if="sort_places && searchResultsOn === false"
        v-for="(place_marker, id) in places"
        :lat-lng="place_marker.coords"
        @click="showPlaceDescription(place_marker.id)"
    >
      <l-icon :icon-url="placeMarkerIconUrl" :icon-size="[100, 100]"></l-icon>
    </l-marker>
    <l-marker v-if="sort_places && searchResultsOn === true"
              v-for="(place_marker, id) in searchResultsPlaces"
              :lat-lng="place_marker.coords"
              @click="showPlaceDescription(place_marker.id)"
    >
      <l-icon :icon-url="placeMarkerIconUrl" :icon-size="[100, 100]"></l-icon>
    </l-marker>


    <l-marker v-if="sort_organizations && searchResultsOn === false"
        v-for="(organization_marker, id) in organizations"
        :lat-lng="organization_marker.coords"
        @click="showOrganizationDescription(organization_marker.id)"
    >
      <l-icon :icon-url="organizationMarkerIconUrl" :icon-size="[100, 100]"></l-icon>
    </l-marker>
    <l-marker v-if="sort_organizations && searchResultsOn === true"
              v-for="(organization_marker, id) in searchResultsOrganizations"
              :lat-lng="organization_marker.coords"
              @click="showOrganizationDescription(organization_marker.id)"
    >
      <l-icon :icon-url="organizationMarkerIconUrl" :icon-size="[100, 100]"></l-icon>
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
import OrganizationWindow from "@/components/OrganizationWindow";
import 'leaflet/dist/leaflet.css';

export default {
  name: "IntMap",
  emits: ['event_create', 'event_delete', 'event_add_form_close',
    'take_a_part', 'un_part', 'save_edit_to_app', 'place_create',
    'place_delete', 'place_add_form_close', 'save_place_to_app',
    'save_organization_to_app', 'organization_delete', 'organization_add_form_close',
    'organization_create'
  ],
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
    sort_events : {
      type: Boolean
    },
    sort_places : {
      type: Boolean
    },
    sort_organizations : {
      type: Boolean
    },
    searchResultsOn : {
      type: Boolean
    },
    searchTerm : {
      type: String
    },
    searchResultsEvents: {
      type: Array,
      required: true
    },
    searchResultsPlaces: {
      type: Array,
      required: true
    },
    searchResultsOrganizations: {
      type: Array,
      required: true
    },
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
    EventMap,
    PlaceWindow,
    OrganizationWindow,
    LIcon,
  },
  data() {
    return {
      showEvent: true,
      showPlace: true,
      showOrganization: true,
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
      selectedOrganization: null,
      imageUrl: '',
      new_place: {
        id: '',
        name: '',
        description: '',
        coord_x: '',
        coord_y: '',
        coords: '',
        photo: [],
        address: '',
        place_type: '',
        selectedPlace: null,
      },
      new_organization: {
        id: '',
        name: '',
        description: '',
        coord_x: '',
        coord_y: '',
        coords: '',
        photo: [],
        address: '',
        selectedOrganization: null,
      },
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
        souls_count: 0,
        link: '',
        selectedEvent: null,
        coord_x: '',
        coord_y: '',
        coords: '',
      },
    };
  },
  methods : {
    handleFileUploadEvent(event) {
      const files = event.target.files;
      for (let i = 0; i < files.length; i++) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.new_event.photo.push({ url: e.target.result });
        };
        reader.readAsDataURL(files[i]);
      }
    },
    handleFileUploadPlace(event) {
      const files = event.target.files;
      for (let i = 0; i < files.length; i++) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.new_place.photo.push({ url: e.target.result });
        };
        reader.readAsDataURL(files[i]);
      }
    },
    handleFileUploadOrganization(event) {
      const files = event.target.files;
      for (let i = 0; i < files.length; i++) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.new_organization.photo.push({ url: e.target.result });
        };
        reader.readAsDataURL(files[i]);
      }
    },
    saveNewEventData (edit_event, buff_id) {
      this.$emit('save_edit_to_app', edit_event, buff_id);
    },
    saveNewPlaceData (edit_place, buff_id) {
      this.$emit('save_place_to_app', edit_place, buff_id);
    },
    saveNewOrganizationData (edit_place, buff_id) {
      this.$emit('save_organization_to_app', edit_place, buff_id);
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

      this.new_organization.coord_x = this.markerPosition[0];
      this.new_organization.coord_y = this.markerPosition[1];
      this.new_organization.coords = this.markerPosition;

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
          souls_count: 0,
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
    organization_add() {
      if (this.new_organization.coord_x && this.new_organization.coord_y) {
        this.new_organization.id = Date.now();
        this.$emit('organization_create', this.new_organization);
        this.new_organization = {
          name: '',
          description: '',
          coord_x: '',
          coord_y: '',
          coords: '',
          photo: '',
          selectedOrganization : null,
          address: '',
        }
        this.new_organization.photo = '';
        this.$emit('organization_add_form_close');
      }
    },
    event_down(id) {
      this.$emit('event_delete', id);
    },
    place_down(id) {
      this.$emit('place_delete', id);
    },
    organization_down(id) {
      this.$emit('organization_delete', id);
    },
    showEventDescription(id) {
      this.showPlace = false;
      this.showOrganization = false;
      this.selectedEvent = id;
      this.showEvent = true;
      this.event_item(this.selectedEvent);
    },
    showPlaceDescription(id) {
      this.showEvent = false;
      this.showOrganization = false;
      this.selectedPlace = id;
      this.showPlace = true;
      this.place_item(this.selectedPlace);
    },
    showOrganizationDescription(id) {
      this.showEvent = false;
      this.showPlace = false;
      this.selectedOrganization = id;
      this.showOrganization = true;
      this.organization_item(this.selectedOrganization);
    },
    eventClose() {
      this.showEvent = false;
    },
    placeClose() {
      this.showPlace = false;
    },
    organizationClose() {
      this.showOrganization = false;
    },
    event_item(selectedId) {
      return this.events.find(item => item.id === selectedId);
    },
    place_item(selectedId) {
      return this.places.find(item => item.id === selectedId);
    },
    organization_item(selectedId) {
      return this.organizations.find(item => item.id === selectedId);
    },
    closeEventAddFormEmit() {
      this.$emit('event_add_form_close');
    },
    closePlaceAddFormEmit() {
      this.$emit('place_add_form_close');
    },
    closeOrganizationAddFormEmit() {
      this.$emit('organization_add_form_close');
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