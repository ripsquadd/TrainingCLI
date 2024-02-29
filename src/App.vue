<template>
  <top-bar
      @event_add_form_show="eventFormShow"
  />
  <div class="content">
    <int-map :events="events"
             :event_form_on="event_form_on"
             @create="create_event"
             @event_delete="event_teleport_to_death"
             @event_add_form_close="eventFormClose"
    />
  </div>
</template>

<script>
  import EventAdd from "@/components/EventAdd";
  import EventMap from "@/components/EventMap";
  import IntMap from "@/components/IntMap";
  import TopBar from "@/components/TopBar";
  export default {
    name: "App",
    components: {
      TopBar,
      EventAdd, EventMap, IntMap
    },
    data () {
      return {
        events: [],
        event_form_on : false,
      }
    },
    methods: {
      eventFormShow() {
        this.event_form_on = true;
      },
      eventFormClose() {
        this.event_form_on = false;
      },
      create_event(new_event) {
        this.events.push(new_event)
        this.save();
        console.log(this.events)
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
  @font-face {
    font-family: "TF2 Build";
    src: url("../public/assets/tf2build.ttf");
  }
  #app {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    font-family: "TF2 Build", Serif;
  }
  .content {
    display: flex;
  }
  body {
    background: #362d26;
  }
  button {
    cursor: pointer;
    font-family: "TF2 Build", Serif;
  }
</style>