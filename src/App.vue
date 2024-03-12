<template>
  <top-bar v-if="top_bar_on"
      @event_add_form_show="eventFormShow"
      @place_add_form_show="placeFormShow"
      @organization_add_form_show="organizationFormShow"
  />
  <div class="content">
    <int-map :events="events"
             :places="places"
             :organizations="organizations"
             :event_form_on="event_form_on"
             :place_form_on="place_form_on"
             :organization_form_on="organization_form_on"
             @event_create="create_event"
             @event_delete="event_teleport_to_death"
             @save_edit_to_app="saveEvent"
             @event_add_form_close="eventFormClose"
             @take_a_part="takePart"
             @un_part="unPart"
             @place_create="create_place"
             @place_delete="place_teleport_to_death"
             @place_add_form_close="placeFormClose"
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
  import PlaceWindow from "@/components/PlaceWindow";
  export default {
    name: "App",
    components: {
      TopBar, EventAdd, EventMap, IntMap, CustomFooter, PlaceWindow
    },
    data () {
      return {
        events: [],
        places: [],
        organizations: [],
        top_bar_on: true,
        event_form_on: false,
        place_form_on: false,
        organization_form_on: false,
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
      placeFormShow() {
        this.top_bar_on = false;
        this.place_form_on = true;
      },
      placeFormClose() {
        this.top_bar_on = true;
        this.place_form_on = false;
      },
      organizationFormShow() {
        this.top_bar_on = false;
        this.organization_form_on = true;
      },
      organizationFormClose() {
        this.top_bar_on = true;
        this.organization_form_on = false;
      },
      create_event(new_event) {
        this.events.push(new_event)
        this.save();
      },
      create_place(new_place) {
        this.places.push(new_place)
        this.save();
      },
      create_organization(new_organization) {
        this.organizations.push(new_organization)
        this.save();
      },

      event_teleport_to_death(id) {
        this.events.splice(id, 1);
        this.save();
      },
      place_teleport_to_death(id) {
        this.places.splice(id, 1);
        this.save();
      },
      save() {
        localStorage.events = JSON.stringify(this.events);
        localStorage.places = JSON.stringify(this.places);
        localStorage.organizations = JSON.stringify(this.organizations);
      }
    },
    mounted() {
      if (localStorage.events) {
        this.events = JSON.parse(localStorage.events)
      }
      if (localStorage.places) {
        this.places = JSON.parse(localStorage.places)
      }
      if (localStorage.organizations) {
        this.organizations = JSON.parse(localStorage.organizations)
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