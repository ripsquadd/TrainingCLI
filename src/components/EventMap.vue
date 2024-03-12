<template>
  <div class="event-map" ref="eventmap">
    <div class="events" v-for="(event, id) in events">

      <div  class="event-edit" v-if="event.id === selectedEvent && event_edit_form === true">
        <div class="close-btn">
          <button @click="cancel_event">X</button>
        </div>
        <section>Редактировать событие</section>
        <div>
          <label for="photo" class="input-file">Загрузить изабражения</label>
          <input type="file" name="eventPhoto" id="photo" accept="image/png, image/jpeg, image/jpg"
                 @change="FileUpload" style="display: none;">
        </div>
        <div>
          <label for="title" >Название события</label>
          <input type="text" name="eventTitle" id="title" v-model="edit_event.title">
        </div>
        <div>
          <label for="place">Место проведения</label>
          <input type="text" name="eventPlace" id="place" v-model="edit_event.place">
        </div>
        <div>
          <label for="date">Дата проведения</label>
          <input type="date" name="eventDate" id="date" v-model="edit_event.date">
        </div>
        <div>
          <label for="time">Время проведения</label>
          <input type="time" name="eventTime" id="time" v-model="edit_event.time">
        </div>
        <div>
          <label for="short_detail">Краткое описание</label>
          <input type="text" name="eventShortDetail" id="short_detail" v-model="edit_event.short_detail">
        </div>
        <div>
          <label for="age_rating">Возрастное ограничение</label>
          <input type="text" name="eventAgeRating" id="age_rating" v-model="edit_event.age_rating">
        </div>
        <div>
          <label for="event_type">Тип события</label>
          <input type="text" name="eventType" id="event_type" v-model="edit_event.event_type">
        </div>
        <div>
          <label for="event_tags">Тэги</label>
          <input type="text" name="eventTags" id="event_tags" v-model="edit_event.event_tags">
        </div>
        <div>
          <label for="link">Ссылка</label>
          <input type="text" name="eventLink" id="link" v-model="edit_event.link">
        </div>
        <button type="submit" @click="save_event">Сохранить изменения</button>
        <button type="submit" @click="cancel_event">Отменить изменения</button>
      </div>


      <div class="event-item"  v-if="event.id === selectedEvent && event_edit_form === false">
        <div class="close-btn">
          <button @click="closeEventEmit">X</button>
        </div>
        <div class="img-content">
          <img :src="event.photo">
        </div>
        <p class="event-title">{{event.title}}</p>
        <div class="first">Место проведения: <p class="second">{{event.place}}</p></div>
        <div class="first">Дата проведения: <p class="second">{{event.date}}</p></div>
        <div class="first">Время проведения: <p class="second">{{event.time}}</p></div>
        <div class="first">Краткое описание: <p class="second">{{event.short_detail}}</p></div>
        <div class="first">Возрастное ограничение: <p class="second">{{event.age_rating}}</p></div>
        <div class="first">Тип события: <p class="second">{{event.event_type}}</p></div>
        <div class="first">Тэги: <p class="second">{{event.event_tags}}</p></div>
        <div class="first">Количество участвующих: <p class="second">{{event.souls_count}}</p></div>
        <p class="link-content">Ссылка: <a href="{{event.link}}">{{event.link}}</a> </p>
        <button @click="eventTakePart(id)">Участвовать</button>
        <button @click="eventUnPart(id)">Перестать участвовать</button>
        <button @click="startEventEdit(id)">Редактировать событие</button>
        <button @click="eventDelete(id)">Удалить событие</button>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "EventMap",
  emits: ['event_transfer_to_death', 'event_close', 'event_taking_part', 'event_un_parting', 'save_event_edit'],
  props: {
    events: {
      type: Array,
      required: true
    },
    selectedEvent : {

    },
  },
  data() {
    return {
      edit_event: {
        photo: [],
        title: '',
        id: '',
        place: '',
        date: '',
        time: '',
        short_detail: '',
        age_rating: '',
        event_type: '',
        event_tags: '',
        link: '',
        coord_x: '',
        coord_y: '',
        coords: '',
      },
      event_edit_form: false,
      buffId: '',
    }
  },
  methods: {
    eventTakePart(id) {
      this.$emit('event_taking_part', id);
    },
    eventUnPart(id) {
      this.$emit('event_un_parting', id);
    },
    eventDelete(id) {
      this.$emit('event_transfer_to_death', id);
    },
    closeEventEmit() {
      this.$emit('event_close');
    },
    FileUpload(event) {
      const file = event.target.files[0];
      let buff = URL.createObjectURL(file);
      this.edit_event.photo.push(buff)
    },
    cancel_event() {
      this.event_edit_form = !this.event_edit_form;
      this.buffId = '';

      this.edit_event.id = '';
      this.edit_event.photo = '';
      this.edit_event.title = '';
      this.edit_event.place = '';
      this.edit_event.date = '';
      this.edit_event.time = '';
      this.edit_event.short_detail = '';
      this.edit_event.age_rating = '';
      this.edit_event.event_type = '';
      this.edit_event.event_tags = '';
      this.edit_event.link = '';
      this.edit_event.coord_x = '';
      this.edit_event.coord_y = '';
      this.edit_event.coords = '';
    },
    startEventEdit(id) {
      this.event_edit_form = !this.event_edit_form;
      this.buffId = id;

      this.edit_event.id = this.events[id].id;
      this.edit_event.photo = this.events[id].photo;
      this.edit_event.title = this.events[id].title;
      this.edit_event.place = this.events[id].place;
      this.edit_event.date = this.events[id].date;
      this.edit_event.time = this.events[id].time;
      this.edit_event.short_detail = this.events[id].short_detail;
      this.edit_event.age_rating = this.events[id].age_rating;
      this.edit_event.event_type = this.events[id].event_type;
      this.edit_event.event_tags = this.events[id].event_tags;
      this.edit_event.link = this.events[id].link;
      this.edit_event.coord_x = this.events[id].coord_x;
      this.edit_event.coord_y = this.events[id].coord_y;
      this.edit_event.coords = this.events[id].coords;
    },
    save_event() {
      this.$emit('save_event_edit', this.edit_event, this.buffId);
      this.event_edit_form = !this.event_edit_form;
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