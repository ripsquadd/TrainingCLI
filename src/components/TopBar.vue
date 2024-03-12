<template>
  <div class="pre-nav">
    <a href="#">
      <img class="logo"
           :src="imageSrc"
           @mouseover="changeImage"
           @mouseleave="restoreImage"
           alt="logo">
    </a>
    <nav>
      <div>
        <button @click="toggleCreateMenu">Создать</button>
        <ul v-if="showCreateMenu">
          <li><button @click="showEventAddFormEmit">Событие</button></li>
          <li><button @click="showPlaceAddFormEmit">Место</button></li>
          <li><button @click="showOrganizationAddFormEmit">Организацию</button></li>
        </ul>
      </div>
      <div>
        <button @click="toggleSortMenu">Показать</button>
        <ul v-if="showSortMenu">
          <li><button>Всё</button></li>
          <li><button>Событие</button></li>
          <li><button>Место</button></li>
          <li><button>Организацию</button></li>
        </ul>
      </div>
      <div>
        <button>Авторизация</button>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: "TopBar",
  emits: ['event_add_form_show', 'place_add_form_show', 'organization_add_form_show'],
  data() {
    return {
      originalImageSrc: require("./components_assets/logo.png"),
      hoverImageSrc: require("./components_assets/logo_alt.png"),
      imageSrc: require("./components_assets/logo.png"),
      showCreateMenu: false,
      showSortMenu: false,
    };
  },
  methods: {
    changeImage() {
      this.imageSrc = this.hoverImageSrc;
    },
    restoreImage() {
      this.imageSrc = this.originalImageSrc;
    },
    toggleCreateMenu() {
      this.showCreateMenu = !this.showCreateMenu;
      this.showSortMenu = false;
    },
    toggleSortMenu() {
      this.showSortMenu = !this.showSortMenu;
      this.showCreateMenu = false;
    },
    showEventAddFormEmit () {
      this.showCreateMenu = !this.showCreateMenu;
      this.$emit('event_add_form_show');
    },
    showPlaceAddFormEmit() {
      this.showCreateMenu = !this.showCreateMenu;
      this.$emit('place_add_form_show');
    },
    showOrganizationAddFormEmit() {
      this.showCreateMenu = !this.showCreateMenu;
      this.$emit('organization_add_form_show');
    }
  }
}
</script>

<style scoped>
nav {
  display: flex;
  background: #1B1212;
  flex-direction: row;
  align-items: stretch;
  justify-content: space-between;
  padding: 10px;
  border-radius: 5px;
  font-family: "TF2 Build", Serif;
}
button {
  border: none;
  background: #1B1212;
  font-size: 20pt;
  color: #FFA500 ;
  text-shadow: #FF5733 1px 0 15px;
}
button:hover {
  color: #FF69B4;
  text-shadow: #FF10F0 1px 0 20px;
}
ul {
  list-style-type: none;
  padding-left: 0;
}
.logo {
  width: 128px;
  height: 128px;
}
.pre-nav {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  position: absolute;
  z-index: 1;
  left: 5%
}
</style>