<template>
  <nav>
    <button>Создать событие</button>
    <button>Отменить</button>
    <button>События</button>
  </nav>
</template>

<script>
export default {
  name: "TopBar"
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
nav > button {
  border: none;
  background: #1B1212;
  font-size: 20pt;
  color: #FFA500 ;
  text-shadow: #FF5733 1px 0 15px;
}
nav > button:hover {
  color: #FF69B4;
  text-shadow: #FF10F0 1px 0 20px;
}
</style>