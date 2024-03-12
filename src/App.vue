<template>
  <top-bar v-if="top_bar_on"
      @event_add_form_show="eventFormShow"
  />
  <div class="content">
    <int-map :events="events"
             :event_form_on="event_form_on"
             @create="create_event"
             @event_delete="event_teleport_to_death"
             @event_add_form_close="eventFormClose"
             @take_a_part="takePart"
             @un_part="unPart"
             @save_edit_to_app="saveEvent"
    />
  </div>
  <custom-footer/>
</template>

<script>
  import EventAdd from "@/components/EventAdd";
  import EventMap from "@/components/EventMap";
  import IntMap from "@/components/IntMap";
  import TopBar from "@/components/TopBar";
  import CustomFooter from "@/components/CustomFooter";
  export default {
    name: "App",
    components: {
      TopBar, EventAdd, EventMap, IntMap, CustomFooter
    },
    data () {
      return {
        events: [],
        top_bar_on: true,
        event_form_on : false,
      }
    },
    methods: {
      saveEvent(edit_event, buff_id) {
      this.events[buff_id] = edit_event;
      this.save();
      },
      takePart(id) {
        this.events[id].souls_count +=1;
        this.save();
      },
      unPart(id) {
        if (this.events[id].souls_count === 0) {
          console.log('You already not part of event');
        }
        else {
          this.events[id].souls_count -=1;
        }
        this.save();
      },
      eventFormShow() {
        this.top_bar_on = false;
        this.event_form_on = true;
      },
      eventFormClose() {
        this.top_bar_on = true;
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
    overflow-x:hidden;
  }
  button {
    cursor: pointer;
    font-family: "TF2 Build", Serif;
  }
</style>