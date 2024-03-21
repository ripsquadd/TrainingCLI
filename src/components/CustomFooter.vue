<template>
<div class="custom-footer">
  <p class="title">Spotlight Prototype Close Beta 0.5.3</p>

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
            <li>1. Интеграция OpenStreetMap</li>
            <li>2. События: создание, редактирование и удаление</li>
            <li>3. Места: создание, редактирование и удаление</li>
            <li>4. Организации: создание, редактирование и удаление</li>
            <li>5. Получение координат для будущего события, места или организации через клик по карте</li>
            <li>6. Возможность получить информацию о событии, месте или организации при клике на маркер</li>
            <li>7. Кнопка участвовать (в прототипе пока как счетчик только)</li>
            <li>8. Сортировка</li>
            <li>9. Поиск</li>
            <li>10. Карусель изображений</li>
          </ul>
        </div>
      </div>

      <div class="container">
        <div>
          <p class="title">Что скоро будет реализовано:</p>
          <ul>
            <li>Регистрация</li>
            <li>Авторизация</li>
            <li>Убрать ебучий флаг с карты, а то пизда на защите</li>
            <li>Рефакторинг кода</li>
          </ul>
        </div>
      </div>
    </div>
    <div class="line">
      <div class="container">
        <div>
          <p class="title">Известные баги:</p>
          <ul>
            <li>Проблема с id: вроде как проблема с самими id от vue (проблема решена)</li>
            <li>Редактирование экземпляров сущности, вызванных поиском,
              применяется только после пустого значения в поиске</li>
            <li>Failed to set a named property 'events' on 'Storage':
              Setting the value of 'events' exceeded the quota.
              - проблема на стороне браузера из-за лимитов стораджа,
              на конечный продукт не повляет, тк там в сторадже храниться если что-то и будет,
              то значение переменной темы </li>
          </ul>
        </div>
      </div>

      <div class="container">
        <div>
          <p class="title">Возникшие вопросы:</p>
          <ul>
            <li>Код дерьмо?</li>
            <li>Сложно ли его будет переделать для работы с Api?</li>
            <li>Стилизация карты пока затруднительно</li>
            <li>Соответственно ночная тема пока в пролёте</li>
            <li>Редактирование координат событий пока не допёр как сделать</li>
          </ul>
        </div>
      </div>

      <div class="container">
        <div>
          <p class="title">Идеи:</p>
          <ul>
            <li>Ночная тема</li>
            <li>Человеческий вывод даты</li>
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