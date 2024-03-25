<template>
  <div class="event-map" ref="eventmap">
    <div class="events" v-for="(organization, id) in organizations">

      <div  class="event-edit" v-if="organization.id === selectedOrganization && organization_edit_form === true">
        <div class="close-btn">
          <button @click="cancel_organization">X</button>
        </div>
        <section>Редактировать организацию</section>
        <div>
          <label for="photo" class="input-file">Загрузить изображения</label>
          <input type="file" id="photo" multiple @change="handleFileUploadEditOrganization" style="display: none">
        </div>
        <div>
          <Carousel style="width: 400px">
            <Slide v-for="image in edit_organization.photo" :key="image">
              <img class="carousel__item" :src="image.url" alt="Uploaded Image">
            </Slide>

            <template #addons>
              <Navigation />
              <Pagination />
            </template>
          </Carousel>
        </div>
        <div>
          <label for="title" >Название организации</label>
          <input type="text" name="eventTitle" id="title" v-model="edit_organization.name">
        </div>
        <div>
          <label for="place">Описание организации</label>
          <input type="text" name="eventPlace" id="place" v-model="edit_organization.description">
        </div>
        <div>
          <label for="placeAddress">Адрес</label>
          <input type="text" name="placeAddress" id="place_address" v-model="edit_organization.address">
        </div>
        <button type="submit" @click="save_organization">Сохранить изменения</button>
        <button type="submit" @click="cancel_organization">Отменить изменения</button>
      </div>


      <div class="event-item"  v-if="organization.id === selectedOrganization && organization_edit_form === false">
        <div class="close-btn">
          <button @click="closeOrganizationEmit">X</button>
        </div>
        <div>
          <Carousel style="width: 400px">
            <Slide v-for="image in organization.photo" :key="image">
              <img class="carousel__item" :src="image.url" alt="Uploaded Image">
            </Slide>

            <template #addons>
              <Navigation />
              <Pagination />
            </template>
          </Carousel>
        </div>
        <p class="event-title">{{organization.name}}</p>
        <div class="first">Описание: <p class="second">{{organization.description}}</p></div>
        <div class="first">Адрес: <p class="second">{{organization.address}}</p></div>
        <button v-if="userLogin" @click="startOrganizationEdit(id)">Редактировать организацию</button>
        <button v-if="userLogin" @click="organizationDelete(id)">Удалить организацию</button>
      </div>
    </div>

  </div>
</template>

<script>
import 'vue3-carousel/dist/carousel.css'
import { Carousel, Slide, Pagination, Navigation } from 'vue3-carousel'
export default {
  name: "OrganizationWindow",
  components: {
    Carousel,
    Slide,
    Pagination,
    Navigation,
  },
  emits: ['organization_transfer_to_death', 'organization_close', 'save_organization_edit'],
  props: {
    organizations: {
      type: Array,
      required: true
    },
    userLogin : {
      type: Boolean
    },
    selectedOrganization : {

    },
  },
  data() {
    return {
      edit_organization: {
        photo: '',
        id: '',
        name: '',
        description: '',
        coord_x: '',
        coord_y: '',
        coords: '',
        address: '',
      },
      organization_edit_form: false,
      buffId: '',
    }
  },
  methods: {
    handleFileUploadEditOrganization(event) {
      const files = event.target.files;
      for (let i = 0; i < files.length; i++) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.edit_organization.photo.push({ url: e.target.result });
        };
        reader.readAsDataURL(files[i]);
      }
    },
    organizationDelete(id) {
      this.$emit('organization_transfer_to_death', id);
    },
    closeOrganizationEmit() {
      this.$emit('organization_close');
    },
    cancel_organization() {
      this.organization_edit_form = !this.organization_edit_form;
      this.buffId = '';

      this.edit_organization.id = '';
      this.edit_organization.photo = '';
      this.edit_organization.name = '';
      this.edit_organization.description = '';
      this.edit_organization.address = '';
      this.edit_organization.coord_x = '';
      this.edit_organization.coord_y = '';
      this.edit_organization.coords = '';
    },
    startOrganizationEdit(id) {
      this.organization_edit_form = !this.organization_edit_form;
      this.buffId = id;

      this.edit_organization.id = this.organizations[id].id;
      this.edit_organization.photo = this.organizations[id].photo;
      this.edit_organization.name = this.organizations[id].name;
      this.edit_organization.description = this.organizations[id].description;
      this.edit_organization.address = this.organizations[id].address;
      this.edit_organization.coord_x = this.organizations[id].coord_x;
      this.edit_organization.coord_y = this.organizations[id].coord_y;
      this.edit_organization.coords = this.organizations[id].coords;
    },
    save_organization() {
      this.$emit('save_organization_edit', this.edit_organization, this.buffId);
      this.organization_edit_form = !this.organization_edit_form;
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
.carousel__item {
  min-height: 200px;
  background-color: var(--vc-clr-primary);
  color: var(--vc-clr-white);
  font-size: 20px;
  border-radius: 8px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.carousel__slide {
  padding: 10px;
}

.carousel__prev,
.carousel__next {
  box-sizing: content-box;
  border: 5px solid white;
}
</style>