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
</template>

<script>

export default {
  name: "EventAdd",
  emits: ['create', 'coords'],
  props: {
    form_on: {
      type: Number
    },
  },
  data() {
    return {
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
      buffx: 0,
      buffy: 0
    }
  },
  methods: {
    coords(x, y) {
      console.log(x, y)
    },
    FileUpload(event) {

      // const files = [];
      // files.push(URL.createObjectURL(event.target.files))
      // this.new_event.photo.push(files)

      const file = event.target.files[0];
      let buff = URL.createObjectURL(file);
      this.new_event.photo.push(buff)

      // const file = event.dataTransfer.files;
    },
    // send_coords(x, y) {
    //   this.buffx = x;
    //   this.buffy = y;
    //   console.log(this.buffx)
    // },
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
      // this.form_on = 0;
    },
  }
}
</script>

<style scoped>
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