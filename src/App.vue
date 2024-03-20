<template>
  <top-bar v-if="top_bar_on"
      @event_add_form_show="eventFormShow"
      @place_add_form_show="placeFormShow"
      @organization_add_form_show="organizationFormShow"
      @toggle_all="allShow"
      @toggle_events="eventsShow"
      @toggle_places="placesShow"
      @toggle_organizations="organizationsShow"
      @send_search_query="searchTermUpdate"
  />
  <div class="content">
    <int-map :events="events"
             :places="places"
             :organizations="organizations"
             :event_form_on="event_form_on"
             :place_form_on="place_form_on"
             :organization_form_on="organization_form_on"
             :sort_events="sort_events"
             :sort_places="sort_places"
             :sort_organizations="sort_organizations"
             :searchTerm="searchTerm"
             :searchResultsOn="searchResultsOn"
             :searchResultsEvents="searchResultsEvents"
             :searchResultsPlaces="searchResultsPlaces"
             :searchResultsOrganizations="searchResultsOrganizations"
             @event_create="create_event"
             @place_create="create_place"
             @organization_create="create_organization"
             @event_delete="event_teleport_to_death"
             @place_delete="place_teleport_to_death"
             @organization_delete="organization_teleport_to_death"
             @save_edit_to_app="saveEvent"
             @save_place_to_app="savePlace"
             @save_organization_to_app="saveOrganization"
             @event_add_form_close="eventFormClose"
             @place_add_form_close="placeFormClose"
             @organization_add_form_close="organizationFormClose"
             @take_a_part="takePart"
             @un_part="unPart"
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
  import OrganizationWindow from "@/components/OrganizationWindow";
  export default {
    name: "App",
    components: {
      TopBar, EventAdd, EventMap, IntMap, CustomFooter, PlaceWindow, OrganizationWindow
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
        sort_events: true,
        sort_places: true,
        sort_organizations: true,
        searchTerm: '',
        searchResultsOn: false,
        searchResultsEvents: [],
        searchResultsPlaces: [],
        searchResultsOrganizations: [],
      }
    },
    methods: {
      saveEvent(edit_event, buff_id) {
        this.events[buff_id] = edit_event;
        this.save();
      },
      savePlace(edit_place, buff_id) {
        this.places[buff_id] = edit_place;
        this.save();
      },
      saveOrganization(edit_organization, buff_id) {
        this.organizations[buff_id] = edit_organization;
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
      allShow() {
        this.sort_events = true;
        this.sort_places = true;
        this.sort_organizations = true;
      },
      eventsShow() {
        this.sort_events = true;
        this.sort_places = false;
        this.sort_organizations = false;
      },
      placesShow() {
        this.sort_events = false;
        this.sort_places = true;
        this.sort_organizations = false;
      },
      organizationsShow() {
        this.sort_events = false;
        this.sort_places = false;
        this.sort_organizations = true;
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
      organization_teleport_to_death(id) {
        this.organizations.splice(id, 1);
        this.save();
      },
      searchTermUpdate(searchQuery) {
        this.searchTerm = searchQuery;
        if (this.searchTerm !== '') {
          this.searchResultsEvents = this.events.filter(obj => obj.title.toLowerCase().includes(this.searchTerm.toLowerCase()));
          this.searchResultsPlaces = this.places.filter(obj => obj.name.toLowerCase().includes(this.searchTerm.toLowerCase()));
          this.searchResultsOrganizations = this.organizations.filter(obj => obj.name.toLowerCase().includes(this.searchTerm.toLowerCase()));

          this.searchResultsOn = true;
        }
        else {
          this.searchResultsEvents = [];
          this.searchResultsPlaces = [];
          this.searchResultsOrganizations = [];

          this.searchResultsOn = false;
        }
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