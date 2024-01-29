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

    <label for="event_type">Тип мероприятия</label>
    <select name="eventType" id="event_type" v-model="new_event.event_type">
      <option value="1">Гаргулья</option>
      <option value="2">Поношенный пистолет</option>
      <option value="3">Монета</option>
    </select>

    <label for="event_tags">Тэги</label>
    <input type="event_tags" name="eventTags" id="event_tags" v-model="new_event.event_tags">

    <label for="souls_count">Количество участников</label>
    <input type="souls_count" name="eventSoulsCount" id="souls_count" v-model="new_event.souls_count">

    <label for="link">Ссылка</label>
    <input type="link" name="eventLink" id="link" v-model="new_event.link">

    <button type="submit" @click="event_add">Добавить событие</button>
  </div>  <div v-if="form_on === 0" class="event-add">

  <button type="submit" @click="event_add">Добавить событие</button>
</div>
</template>

<script>
export default {
  name: "EventAdd",
  emits: ['create'],
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
      },
    }
  },
  methods: {
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
</style>