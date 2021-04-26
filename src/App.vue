<template>
  <div class="register flex flex-d-column a-i-center">
    <span class="register-title">Регистрация</span>
    <form class="register-form">
      <div class="register-formsItem flex">
        <label for="name">Имя</label><br>
        <input type="text" id="name" name="name"><br>
      </div>
      <div class="register-formsItem flex">
        <label for="surname">Фамилия</label><br>
        <input type="text" id="surname" name="surname"><br>
      </div>
      <div class="register-formsItem flex">
        <label for="patronymic">Отчество:</label><br>
        <input type="text" id="patronymic" name="patronymic"><br>
      </div>
      <div class="register-formsItem flex">
        <label for="birthday">Дата рождения:</label><br>
        <input type="date" id="birthday" name="date"><br>
      </div>
      <div class="register-formsItem flex">
        <label for="phoneNumber">Номер телефона</label><br>
        <input type="number" id="phoneNumber" name="phoneNumber"><br>
      </div>
      <div class="register-formsItem flex">
        <span>Выберите пол:</span>
        <input type="radio" id="male" name="gender" value="male">
        <label for="male">Мужчина</label><br>
        <input type="radio" id="female" name="gender" value="female">
        <label for="female">Женщина</label><br>
        <input type="radio" id="other" name="gender" value="other">
        <label for="other">Другое</label>
      </div>
      <div class="register-formsItem flex">
        <span>Группа клиентов</span>
        <input type="checkbox" id="vip" name="clientsGroup" value="vip">
        <label for="male">VIP</label><br>
        <input type="checkbox" id="problem" name="clientsGroup" value="problem">
        <label for="female">Проблемные</label><br>
        <input type="checkbox" id="oms" name="clientsGroup" value="problem">
        <label for="other">ОМС</label>
      </div>
      <div class="register-formsItem flex">
        <span>Лечащий врач:</span>
        <input type="radio" id="ivanov" name="doctor" value="ivanov">
        <label for="male">Иванов</label><br>
        <input type="radio" id="zaharov" name="doctor" value="zaharov">
        <label for="female">Захаров</label><br>
        <input type="radio" id="chernysheva" name="doctor" value="chernysheva">
        <label for="other">Чернышева</label>
      </div>
      <div class="register-formsItem flex">
        <span>Не отправлять СМС</span>
        <input type="checkbox" id="notSendSms" name="smsCheck" value="notSendSms">
      </div>
      <div class="register-formsItem flex">
        <span>Тип документа</span>
        <select>
          <option>Паспорт</option>
          <option>Свидетельство о рождении</option>
          <option>Вод. удостоверение</option>
        </select>
      </div>
      <div class="register-formsItem flex">
        <label for="phoneNumber">Паспорт</label><br>
        <input type="text" id="serial" name="passportsSerialNumber"><br>
        <input type="text" id="number" name="passportsNumber"><br>
      </div>
      <div class="register-formsItem flex">
        <label for="phoneNumber">Кем выдан</label><br>
        <input type="text" id="who" name="who"><br>
      </div>
      <div class="register-formsItem flex">
        <label for="phoneNumber">Дата выдачи</label><br>
        <input type="text" id="date" name="date"><br>
      </div>

    </form>
  </div>
</template>

<script>
export default {
  name: "App",
};
</script>

<style lang="sass">
body
  background: #f2f2f2
  font-family: Arial

.flex
  display: flex

.flex-d-column
  flex-direction: column

.text-a-center
  text-align: center

.j-c-center
  justify-content: center

.a-i-center
  align-items: center



.register
  margin: 20px auto
  width: 600px
  height: 800px
  border-radius: 19px
  background: #f2f2f2
  box-shadow:  5px 5px 11px rgba(194, 194, 194, 0.5),-8px -8px 11px rgba(255,255,255,1)

  &-title
    margin: 20px 0 0 0
</style>
