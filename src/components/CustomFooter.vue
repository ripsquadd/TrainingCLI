<template>
<div class="custom-footer">
  <p class="title">Spotlight Prototype Beta 0.4.7</p>

  <div class="pre-container">

    <div class="line">
      <div class="container">
        <div>
          <p class="title">О сервисе:</p>
          <p>Spotlight - сервис, представляющий универсальный источник для получения информации
            о культурных мероприятиях и досуговых местах, который позволяет организаторам и посетителям
            находить друг друга через интерактивную карту событий.
          </p>
        </div>
      </div>

      <div class="container">
        <div>
          <p class="title">Что реализовано:</p>
          <ul>
            <li>Интеграция OpenStreetMap</li>
            <li>События: создание, вывод по нажатию на маркер и удаление</li>
            <li>Получение координат для будущего события через клик по карте</li>
            <li>Возможность закрыть компоненты по нажатию на кнопку</li>
            <li>Интерактивное меню с выпадающими списками</li>
            <li>Накинуты дерьмовые стили просто чтобы было проще в этом ориентироваться</li>
          </ul>
        </div>
      </div>

      <div class="container">
        <div>
          <p class="title">Что скоро будет реализовано:</p>
          <ul>
            <li>Счетчик участников</li>
            <li>Редактирование событий</li>
            <li>Места и организации!</li>
            <li>Сортировка!</li>
            <li>Поиск!</li>
            <li>Карусель изображения для ваших событий!</li>
          </ul>
        </div>
      </div>
    </div>
    <div class="line">
      <div class="container">
        <div>
          <p class="title">Известные баги:</p>
          <ul>
            <li>Проблема с id: вроде как проблема с самими id от vue</li>
          </ul>
        </div>
      </div>

      <div class="container">
        <div>
          <p class="title">Возникшие вопросы:</p>
          <ul>
            <li>Код дерьмо?</li>
            <li>Сложно ли его будет переделать для работы с Api?</li>
            <li>Стилизация и изменение маркера пока вызывает трудности</li>
            <li>Стилизация карты пока тоже затруднительно</li>
            <li>Соответственно ночная тема пока в пролёте</li>
            <li>Могу ли я переопределять параметры и стили встроенных в библиотеки компонентов?</li>
          </ul>
        </div>
      </div>

      <div class="container">
        <div>
          <p class="title">Идеи:</p>
          <ul>
            <li>Ночная тема</li>
          </ul>
        </div>
      </div>
    </div>



  </div>
</div>
</template>

<script>
export default {
  name: "CustomFooter"
}
</script>

<style scoped>
.custom-footer {
  background: #1B1212;
  border-radius: 5px;
  width:97vw;
  margin: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.pre-container {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
}
.line {
  display: flex;
  flex-direction: row;
}
.container {
  width: 400px;
}
p {
  color: #C4A484;
  margin: 10px;
}
li {
  color: #C4A484;
}
.title {
  font-size: 20pt;
}
ul {
  padding-left: 0;
}
</style>