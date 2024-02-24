<template>
  <event-map :events="events"
             @event_transfer_to_death="event_teleport_to_death"/>
  <int-map @create="create_event"/>
</template>

<script>
  import EventAdd from "@/components/EventAdd";
  import EventMap from "@/components/EventMap";
  import IntMap from "@/components/IntMap";
  export default {
    name: "App",
    components: {
      EventAdd, EventMap, IntMap
    },
    data () {
      return {
        events: [],
        form_on : 0,
      }
    },
    methods: {
      create_event(new_event) {
        this.events.push(new_event)
        this.save();
      },
      add_form_show() {
        this.form_on = 1;
      },
      add_form_hide() {
        this.form_on = 0;
      },
      event_teleport_to_death(id) {
        this.events.splice(id, 1);
        this.save();
      },
      save() {
        localStorage.event_map = JSON.stringify(this.events);
      }
    },
    mounted() {
      if (localStorage.event_map) {
        this.events = JSON.parse(localStorage.event_map)
      }if (localStorage.event_map) {
        this.events = JSON.parse(localStorage.event_map)
      }
    }
  }
</script>

<style>
  #app {
    display: flex;
    flex-direction: row;
    align-items: flex-start;
  }
</style>