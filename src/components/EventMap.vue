<template>
  <div class="event-map">
    <div class="events" v-for="(event, id) in events">
      <div class="event-item">
        <img :src="event.photo">
        <p class="event-title">{{event.title}}</p>
        <p v-if="event.place">Место проведения: {{event.place}}</p>
        <div class="date" v-if="event.date">
          <p>Дата проведения:</p>
          <p>{{event.date}}</p>
          <p>Время проведения:</p>
          <p>{{event.time}}</p>
        </div>
        <p>Краткое описание: {{event.short_detail}}</p>
        <p>Возрастное ограничение: {{event.age_rating}}</p>
        <div class="type">
          <p>Тип события:</p>
          <p v-if="event.event_type == 1">Гаргулья</p>
          <p v-if="event.event_type == 2">Поношенный пистолет</p>
          <p v-if="event.event_type == 3">Монета</p>
        </div>
        <p>Тэги: {{event.event_tags}}</p>
        <p>Количество участвующих: {{event.souls_count}}</p>
        <p>Ссылки: {{event.link}}</p>
        <button @click="eventDelete(id)">Удалить событие</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EventMap",
  emits: ['event_transfer_to_death'],
  props: {
    events: {
      type: Array,
      required: true
    }
  },
  methods: {
    eventDelete(id) {
      this.$emit('event_transfer_to_death', id);
    },
  }
}
</script>

<style scoped>
  .event-item {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    justify-content: flex-start;
    flex-wrap: nowrap;
    border: 5px solid black;
    border-radius: 5px;
    margin: 10px;
    width: min-content;
    padding: 10px;
  }
</style>