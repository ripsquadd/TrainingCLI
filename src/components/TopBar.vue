<template>
  <nav>
    <div>
      <button @click="toggleCreateMenu">Создать</button>
      <ul v-if="showCreateMenu">
        <li><button @click="showEventAddFormEmit">Событие</button></li>
        <li><button>Место</button></li>
        <li><button>Организацию</button></li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  name: "TopBar",
  emits: ['event_add_form_show'],
  data() {
    return {
      showCreateMenu: false
    };
  },
  methods: {
    toggleCreateMenu() {
      this.showCreateMenu = !this.showCreateMenu;
    },
    showEventAddFormEmit () {
      this.showCreateMenu = !this.showCreateMenu;
      this.$emit('event_add_form_show');
    },
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
</style>