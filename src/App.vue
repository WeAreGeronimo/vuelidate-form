<template>
  <div class="register flex flex-d-column a-i-center">
    <span class="register-title">Регистрация</span>
    <form class="register-form" @submit.prevent="onSubmit()">
      <div class="register-formsItem flex">
        <label
          class="register-labelsName flex a-i-center j-c-flex-end"
          for="surname"
          >Фамилия:</label
        >

        <input
          class="register-inputText"
          type="text"
          id="surname"
          name="surname"
          placeholder="Иванов"
          v-model="form.surname"
          @blur="v$.surname.$touch"
        />
        <div v-if="v$.surname.$error">Name field has an error.</div>
      </div>

      <div class="register-formsItem flex">
        <label
          class="register-labelsName flex a-i-center j-c-flex-end"
          for="name"
          >Имя:</label
        >
        <input
          class="register-inputText"
          placeholder="Александр"
          type="text"
          id="name"
          name="name"
          v-model="form.name"
        />
      </div>

      <div class="register-formsItem flex">
        <label
          class="register-labelsName flex a-i-center j-c-flex-end"
          for="patronymic"
          >Отчество:</label
        >

        <input
          class="register-inputText"
          type="text"
          id="patronymic"
          name="patronymic"
          placeholder="Валентинович"
          v-model="form.patronymic"
        />
      </div>

      <div class="register-formsItem flex">
        <span class="register-labelsName flex a-i-center j-c-flex-end"
          >Выберите пол:</span
        >
        <div>
          <div class="flex">
            <input
              v-model="form.gender"
              class="register-customRadio"
              type="radio"
              id="male"
              name="gender"
              value="male"
            />
            <label for="male">Мужчина</label>
          </div>
          <div class="flex">
            <input
              v-model="form.gender"
              class="register-customRadio"
              type="radio"
              id="female"
              name="gender"
              value="female"
            />
            <label for="female">Женщина</label>
          </div>
          <div class="flex">
            <input
              v-model="form.gender"
              class="register-customRadio"
              type="radio"
              id="other"
              name="gender"
              value="other"
            />
            <label for="other">Другое</label>
          </div>
        </div>
      </div>

      <div class="register-formsItem flex">
        <label
          class="register-labelsName flex a-i-center j-c-flex-end"
          for="birthday"
          >Дата рождения:</label
        >
        <input
          class="register-inputText"
          type="date"
          id="birthday"
          name="date"
          v-model="form.birthday"
        />
      </div>

      <div class="register-formsItem flex">
        <label
          class="register-labelsName flex a-i-center j-c-flex-end"
          for="phoneNumber"
          >Номер телефона:</label
        >
        <label class="register-staticNumber flex a-i-center" for="phoneNumber"
          >+7</label
        >

        <input
          class="register-inputText register-phoneNumber"
          type="number"
          id="phoneNumber"
          name="phoneNumber"
          placeholder="0000000000"
          v-model="form.phoneHolder"
          :onChange="
            (form.phoneNumber = form.countryCode + form.phoneHolder)
          "
        />
      </div>

      <div class="register-formsItem flex">
        <span class="register-labelsName flex a-i-center j-c-flex-end"
          >Группа клиентов</span
        >
        <div>
          <div class="flex">
            <input
              class="register-customCheck"
              type="checkbox"
              id="vip"
              name="clientsGroup"
              value="vip"
            />
            <label for="vip">VIP</label>
          </div>
          <div class="flex">
            <input
              class="register-customCheck"
              type="checkbox"
              id="problem"
              name="clientsGroup"
              value="problem"
            />
            <label for="problem">Проблемные</label>
          </div>
          <div class="flex">
            <input
              class="register-customCheck"
              type="checkbox"
              id="oms"
              name="clientsGroup"
              value="problem"
            />
            <label for="oms">ОМС</label>
          </div>
        </div>
      </div>

      <div class="register-formsItem flex">
        <span class="register-labelsName flex a-i-center j-c-flex-end"
          >Лечащий врач:</span
        >
        <div>
          <div class="flex">
            <input
              class="register-customRadio"
              type="radio"
              id="ivanov"
              name="doctor"
              value="ivanov"
            />
            <label for="ivanov">Иванов</label>
          </div>
          <div class="flex">
            <input
              class="register-customRadio"
              type="radio"
              id="zaharov"
              name="doctor"
              value="zaharov"
            />
            <label for="zaharov">Захаров</label>
          </div>
          <div class="flex">
            <input
              class="register-customRadio"
              type="radio"
              id="chernysheva"
              name="doctor"
              value="chernysheva"
            />
            <label for="chernysheva">Чернышева</label>
          </div>
        </div>
      </div>

      <div class="register-formsItem flex">
        <span class="register-labelsName flex a-i-center j-c-flex-end"
          >Отправлять СМС?</span
        >

        <div>
          <div class="flex">
            <input
              class="register-customRadio"
              type="radio"
              id="notSendSms"
              name="smsCheck"
              value="notSendSms"
            />
            <label for="notSendSms">Нет</label>
          </div>
          <div class="flex">
            <input
              class="register-customRadio"
              type="radio"
              id="sendSms"
              name="smsCheck"
              value="notSendSms"
            />
            <label for="sendSms">Да</label>
          </div>
        </div>
      </div>
      <div class="register-formsItem flex">
        <span class="register-labelsName flex a-i-center j-c-flex-end"
          >Тип документа:</span
        >

        <button
          @click.stop.prevent="dropdownToggle()"
          class="register-typeDocument"
        >
          {{ documentTypeSelected }}
        </button>
        <transition name="fade">
          <div
            v-if="isDropdownOpen"
            @click.stop.prevent="dropdownToggle()"
            class="register-typeDocument register-dropdown flex flex-d-column"
          >
            <span
              v-for="item in documentType"
              :key="item.value"
              @click="selectDocumentType(item.textLabel)"
              >{{ item.textLabel }}</span
            >
          </div>
        </transition>
      </div>

      <div class="register-formsItem flex">
        <label
          class="register-labelsName flex a-i-center j-c-flex-end"
          for="phoneNumber"
          >Паспорт:</label
        >
        <input
          class="register-inputText register-passportSerial"
          type="text"
          id="serial"
          name="passportsSerialNumber"
          placeholder="0000"
        />
        <input
          class="register-inputText register-passportNumber"
          type="text"
          id="number"
          name="passportsNumber"
          placeholder="000000"
        />
      </div>

      <div class="register-formsItem flex">
        <label
          class="register-labelsName flex a-i-center j-c-flex-end"
          for="phoneNumber"
          >Кем выдан:</label
        >

        <input
          class="register-inputText"
          placeholder="УФМС РФ"
          type="text"
          id="who"
          name="who"
        />
      </div>

      <div class="register-formsItem flex">
        <label
          class="register-labelsName flex a-i-center j-c-flex-end"
          for="phoneNumber"
          >Дата выдачи:</label
        >

        <input class="register-inputText" type="date" id="date" name="date" />
      </div>
      <button type="submit" @click="onSubmit()">Зарегистрироваться</button>
    </form>
    <button type="button" @click="test()">test</button>
  </div>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";
export default {
  name: "App",
  data() {
    return {
      isDropdownOpen: false,
      documentTypeSelected: "Паспорт",
      form: {
        name: "",
        surname: "",
        patronymic: "",
        gender: "",
        birthday: "",
        countryCode: "+7",
        phoneHolder: "",
        phoneNumber: "",
      },
      documentType: [
        {
          textLabel: "Паспорт",
          value: "passport",
        },
        {
          textLabel: "Св. о рождении",
          value: "birthCertificate",
        },
        {
          textLabel: "Вод. удостоверение",
          value: "driveLicence",
        },
      ],
    };
  },
  methods: {
    dropdownToggle() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },

    selectDocumentType(documentType) {
      console.log(documentType);
      this.documentTypeSelected = documentType;
    },

    onSubmit() {
      console.log(this.v$.$touch());
      if (this.v$.$error) return;
    },

    test() {
      this.form.correctPhoneNumber =
        this.form.countryCode + this.form.phoneNumber;
      console.log(this.form.correctPhoneNumber);
    },
  },
  setup: () => ({ v$: useVuelidate() }),
  validations: () => ({
    surname: { required },
    lastName: { required },
  }),
};
</script>

<style lang="sass">

@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500&display=swap')

body
  background: #f5f5f5


*
  font-family: 'Roboto', sans-serif
  font-weight: 500


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

.j-c-flex-end
  justify-content: flex-end

.register
  font-size: 16px
  margin: 20px auto
  width: 600px
  border-radius: 19px
  background: #f2f2f2
  box-shadow:  5px 5px 11px rgba(194, 194, 194, 0.5),-8px -8px 11px rgba(255,255,255,0.8)
  padding: 0 40px
  border-left: 1px solid rgba(194,194,194, 0.1)
  border-top: 1px solid rgba(194,194,194, 0.1)
  text-shadow: 2px 2px 2px rgba(151, 150, 150, 0.15)

  &-title
    margin: 10px 0 20px 0
    color: #BABECC
    font-size: 30px
    font-weight: 600
    text-shadow: 1px 1px 1px #fff


  &-inputText
    font-family: 'Montserrat', sans-serif
    font-weight: 400
    font-size: 14px
    border: none
    padding: 10px 20px
    border-radius: 15px
    background: #f5f5f5
    width: 200px
    box-shadow: inset 1px 1px 1px rgba(208, 208, 208, 0.8), inset -1px -1px 1px rgba(255, 255, 255, 0.8)
    transition: all 0.25s ease-out
    &:focus
      outline: none
      padding: 10px 20px
      border-radius: 15px
      background: #f6f6f6
      box-shadow: inset 3px 3px 3px rgba(208, 208, 208, 0.9), inset -3px -3px 3px rgba(255, 255, 255, 0.9)
    &:hover
      border-radius: 15px
      background: #f6f6f6
      box-shadow: inset 3px 3px 3px rgba(208, 208, 208, 0.9), inset -3px -3px 3px rgba(255, 255, 255, 0.9)
    &::placeholder
      opacity: 0.3


  &-passportSerial
    width: 40px
    margin: 0 10px 0 0


  &-passportNumber
    width: 110px

  &-formsItem
    margin: 10px 0
    position: relative

  &-labelsName
    width: 180px
    margin: 0 20px 0 0
    text-shadow: 1px 1px 0 #fff

  &-staticNumber
    font-size: 14px

  &-phoneNumber
    width: 180px
    margin-left: 4px

  &-customRadio
    position: absolute
    z-index: -1
    opacity: 0
    &+label
      font-weight: 400
      display: inline-flex
      align-items: center
      user-select: none
    &+label::before
      content: ''
      width: 20px
      height: 20px
      display: inline-flex
      justify-content: center
      align-items: center
      margin: 2px 6px 2px 0
      background: #f5f5f5
      border-radius: 50%
      box-shadow: inset 1px 1px 2px #d3d3d3, inset -1px -1px 2px #ffffff
    &:not(:disabled):not(:checked)+label:hover::before
      box-shadow: inset 2px 2px 2px #d3d3d3, inset -2px -2px 2px #ffffff
    &:not(:disabled):active+label::before
      background: #d6d6d6
    &:checked+label::before
      content: ""
      border-radius: 50%
      background-image: url("img/2.svg")
      background-size: contain


  &-customCheck
    position: absolute
    z-index: -1
    opacity: 0
    &+label
      display: inline-flex
      align-items: center
      user-select: none
      font-weight: 400
    &+label::before
      content: ''
      width: 20px
      height: 20px
      display: inline-flex
      justify-content: center
      align-items: center
      margin: 2px 6px 2px 0
      background: #f5f5f5
      border-radius: 5px
      box-shadow: inset 1px 1px 2px #d3d3d3, inset -1px -1px 2px #ffffff
    &:not(:disabled):not(:checked)+label:hover::before
      box-shadow: inset 2px 2px 2px #d3d3d3, inset -2px -2px 2px #ffffff
    &:not(:disabled):active+label::before
      background: #d6d6d6
    &:checked+label::before
      content: "✔"
      background: #ffffff
      color: grey



  &-typeDocument
    box-shadow: inset 1px 1px 1px rgba(208, 208, 208, 0.8), inset -1px -1px 1px rgba(255, 255, 255, 0.8)
    font-weight: 400
    border: none
    border-radius: 15px
    padding: 10px 20px
    background: #f5f5f5
    outline: none
    transition: all 1s ease-out
    position: relative
    width: 240px
    &:after
      content: "▼"
      position: absolute
      right: 5%
    &:focus
      color: transparent
    &:hover
      box-shadow:  5px 5px 6px #d3d3d3, -2px -2px 3px #ffffff
    & option
      background: rgba(255,255,255, 0.1)
      box-shadow:  5px 5px 6px #d3d3d3, -2px -2px 3px #ffffff
      border-radius: 10px
    & span
      font-size: 14px
      text-align: center
      padding: 10px 5px
      margin: 2px 0
      transition: all 0.1s ease-in
      font-weight: 400
      &:hover
        border-radius: 15px
        background: #f5f5f5
        cursor: pointer
        box-shadow: inset 3px 3px 3px rgba(208, 208, 208, 0.9), inset -3px -3px 3px rgba(255, 255, 255, 0.9)


  &-dropdown
    padding: 5px 10px
    position: absolute
    z-index: 1
    right: 0%
    width: 220px
    box-shadow:  5px 5px 6px #d3d3d3, -2px -2px 3px #ffffff
    border-left: 1px solid rgba(194,194,194, 0.1)
    border-top: 1px solid rgba(194,194,194, 0.1)
    font-weight: 300
    &:after
      content: ""
      position: absolute
      right: 5%

.fade-enter-active,
.fade-leave-active
  transition: opacity 0.5s ease-out


.fade-enter-from,
.fade-leave-to
  opacity: 0
</style>
