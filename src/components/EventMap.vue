<template>
  <div class="event-map" ref="eventmap">
    <div class="events" v-for="(event, id) in events">
      <div class="event-item"  v-if="event.id === selectedEvent">
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
        <button>Участвовать</button>
        <button>Редактировать событие</button>
        <button @click="eventDelete(id)">Удалить событие</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EventMap",
  emits: ['event_transfer_to_death', 'event_close'],
  props: {
    events: {
      type: Array,
      required: true
    },
    selectedEvent : {

    },
  },
  methods: {
    eventDelete(id) {
      this.$emit('event_transfer_to_death', id);
    },
    closeEventEmit() {
      this.$emit('event_close');
    },
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
    width: max-content;
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
    width: 250px;
  }
  .event-map {
    position: absolute;
    right: 2%;
    top: 16%;
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
</style>