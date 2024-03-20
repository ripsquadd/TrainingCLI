<template>
  <div class="event-map" ref="eventmap">
    <div class="events" v-for="(place, id) in places">

      <div  class="event-edit" v-if="place.id === selectedPlace && place_edit_form === true">
        <div class="close-btn">
          <button @click="cancel_place">X</button>
        </div>
        <section>Редактировать место</section>
        <div>
          <label for="photo" class="input-file">Загрузить изображения</label>
          <input type="file" id="photo" multiple @change="handleFileUploadEditPlace" style="display: none">
        </div>
        <div v-for="(image, index) in edit_place.photo" :key="index">
          <img :src="image.url" alt="Uploaded Image">
        </div>
        <div>
          <label for="title" >Название места</label>
          <input type="text" name="eventTitle" id="title" v-model="edit_place.name">
        </div>
        <div>
          <label for="place">Описание места</label>
          <input type="text" name="eventPlace" id="place" v-model="edit_place.description">
        </div>
        <div>
          <label for="placeAddress">Адрес</label>
          <input type="text" name="placeAddress" id="place_address" v-model="edit_place.address">
        </div>
        <div>
          <label for="placeType">Тип места</label>
          <input type="text" name="placeType" id="place_type" v-model="edit_place.place_type">
        </div>
        <button type="submit" @click="save_place">Сохранить изменения</button>
        <button type="submit" @click="cancel_place">Отменить изменения</button>
      </div>


      <div class="event-item"  v-if="place.id === selectedPlace && place_edit_form === false">
        <div class="close-btn">
          <button @click="closePlaceEmit">X</button>
        </div>
        <div v-for="(file, index) in place.photo" :key="index">
          <img :src="file.url" alt="Uploaded image">
        </div>
        <p class="event-title">{{place.name}}</p>
        <div class="first">Описание: <p class="second">{{place.description}}</p></div>
        <div class="first">Адрес: <p class="second">{{place.address}}</p></div>
        <div class="first">Тип места: <p class="second">{{place.place_type}}</p></div>
        <button @click="startPlaceEdit(id)">Редактировать место</button>
        <button @click="placeDelete(id)">Удалить место</button>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "PlaceWindow",
  emits: ['place_transfer_to_death', 'place_close', 'save_place_edit'],
  props: {
    places: {
      type: Array,
      required: true
    },
    selectedPlace : {

    },
  },
  data() {
    return {
      edit_place: {
        photo: '',
        id: '',
        name: '',
        description: '',
        coord_x: '',
        coord_y: '',
        coords: '',
        address: '',
        place_type: '',
      },
      place_edit_form: false,
      buffId: '',
    }
  },
  methods: {
    handleFileUploadEditPlace(event) {
      const files = event.target.files;
      for (let i = 0; i < files.length; i++) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.edit_place.photo.push({ url: e.target.result });
        };
        reader.readAsDataURL(files[i]);
      }
    },
    placeDelete(id) {
      this.$emit('place_transfer_to_death', id);
    },
    closePlaceEmit() {
      this.$emit('place_close');
    },
    cancel_place() {
      this.place_edit_form = !this.place_edit_form;
      this.buffId = '';

      this.edit_place.id = '';
      this.edit_place.photo = '';
      this.edit_place.name = '';
      this.edit_place.description = '';
      this.edit_place.address = '';
      this.edit_place.place_type = '';
      this.edit_place.coord_x = '';
      this.edit_place.coord_y = '';
      this.edit_place.coords = '';
    },
    startPlaceEdit(id) {
      this.place_edit_form = !this.place_edit_form;
      this.buffId = id;

      this.edit_place.id = this.places[id].id;
      this.edit_place.photo = this.places[id].photo;
      this.edit_place.name = this.places[id].name;
      this.edit_place.description = this.places[id].description;
      this.edit_place.address = this.places[id].address;
      this.edit_place.place_type = this.places[id].place_type;
      this.edit_place.coord_x = this.places[id].coord_x;
      this.edit_place.coord_y = this.places[id].coord_y;
      this.edit_place.coords = this.places[id].coords;
    },
    save_place() {
      this.$emit('save_place_edit', this.edit_place, this.buffId);
      this.place_edit_form = !this.place_edit_form;
    }
  },
}
</script>

<style scoped>
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
.event-item {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: flex-start;
  flex-wrap: nowrap;
  border-radius: 5px;
  margin: 0;
  width: 400px;
  padding: 10px;
  background: #1B1212;
  font-size: 12pt;
}
.event-item > div > p {
  color: #F9F6EE ;
}
.event-item > div {
  color: #C4A484 ;
  display: flex;
}
.event-item > button {
  margin: 5px;
  border: none;
  background: #1B1212;
  color: #FFA500 ;
  text-shadow: #FF5733 1px 0 15px;
  font-size: 14pt;
}
.event-item > button:hover {
  color: #FF69B4;
  text-shadow: #FF10F0 1px 0 20px;
}
.img-content {
  display: flex;
  justify-content: center;
  flex-wrap: nowrap;
  flex-direction: row;
}
img {
  height: 128px;
}
.event-map {
  position: absolute;
  right: 2%;
  top: 7.5%;
  z-index: 1;
}
.event-title {
  font-size: 20pt;
  text-align: center;
  color: #F9F6EE;
}
a {
  color: #FF69B4;
  text-shadow: #FF10F0 1px 0 15px;
}
a:hover {
  color: #FFA500 ;
  text-shadow: #FF5733 1px 0 15px;
}
.first {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: flex-start;
  margin: 0;
}
.second {
  margin: 2px;
  margin-left: 10px;
}
.link-content {
  color: #C4A484;
}
.event-edit {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: flex-start;
  flex-wrap: nowrap;
  border-radius: 5px;
  margin: 0;
  width: 400px;
  padding: 10px;
  background: #1B1212;
  font-size: 12pt;
}
.event-edit > button {
  margin: 5px;
  border: none;
  background: #1B1212;
  color: #FFA500 ;
  text-shadow: #FF5733 1px 0 15px;
  font-size: 14pt;
}
.event-edit > button:hover {
  color: #FF69B4;
  text-shadow: #FF10F0 1px 0 20px;
}
.event-edit > section {
  color: #EADDCA ;
  text-align: center;
  font-size: 20pt;
  margin-bottom: 10px;
}
.event-edit > div > label {
  color: #C4A484;
  padding: 10px;
  width: max-content;
}
.event-edit > div > input {
  color: #EADDCA ;
  background: black;
  border: none;
  border-radius: 5px;
  height: 20px;
}
</style>