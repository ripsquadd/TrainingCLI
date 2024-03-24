<template>
  <div  class="event-add">
    <div class="close-btn">
      <button @click="closeUserLoginFormEmit">X</button>
    </div>
    <section>Вход</section>
    <div>
      <label for="login">Логин</label>
      <input type="text" id="login" v-model="login_user.login">
    </div>
    <div>
      <label for="password">Пароль</label>
      <input type="password" name="password" id="password" v-model="login_user.password">
    </div>
    <button type="submit" @click="user_login">Войти</button>
  </div>
</template>

<script>
export default {
  name: "LoginForm",
  emits: [
    'user_login', 'user_login_form_close'
  ],
  data() {
    return {
      login_user: {
        login: '',
        password: '',
      }
    }
  },
  methods: {
    closeUserLoginFormEmit() {
      this.$emit('user_login_form_close');
    },
    user_login() {
      if (this.login_user.login && this.login_user.password) {
        this.login_user.id = Date.now();
        this.$emit('user_login', this.login_user);
        this.login_user = {
          login: '',
          password: '',
        }
      }
    },
  }
}
</script>

<style scoped>
.event-add {
  display: flex;
  align-items: stretch;
  flex-direction: column;
  margin: 10px;
  border-radius: 5px;
  flex-wrap: nowrap;
  width: -moz-min-content;
  width: min-content;
  padding: 10px;
  background: #1B1212;
  font-size: 14pt;
  justify-content: space-between;
  z-index: 2;
  position: absolute;
  left: 5%;
  top: 6%;
}
.event-add > div {
  display: flex;
  align-items: center;
  align-content: center;
  flex-wrap: nowrap;
}
.event-add > section {
  color: #C4A484; ;
  text-align: center;
  font-size: 20pt;
}
.event-add > div > label {
  color: #C4A484;
  padding: 10px;
  width: max-content;
}
.event-add > div > p {
  color: #C4A484;
  padding: 10px;
}
.event-add > div > input {
  color: #ffffff;
  background: black;
  border: none;
  border-radius: 5px;
  height: 20px;
}
.event-add > button {
  margin: 10px;
  border: none;
  background: #1B1212;
  color: #FFA500 ;
  text-shadow: #FF5733 1px 0 15px;
  font-size: 14pt;
}
.event-add > button:hover  {
  color: #FF69B4;
  text-shadow: #FF10F0 1px 0 20px;
}
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
</style>