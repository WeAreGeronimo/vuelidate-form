<template>
  <div class="register flex flex-d-column a-i-center">
    <span class="register-title">Регистрация</span>
    <div class="register-progress flex">
      <div
        class="register-progressItem flex flex-d-column a-i-center j-c-center"
        v-for="(item, i) in progressBarMap"
        :key="item"
      >
        <div
          class="flex flex-d-column a-i-center"
          v-if="item.name != 'separator'"
        >
          <div class="register-itemName flex text-a-center a-i-center">
            {{ item.name }}
          </div>
          <div class="register-circle flex j-c-center a-i-center">
            {{ item.isComplete ? "" : item.index }}
            <div v-if="item.isComplete" class="register-inlineCircle"></div>
          </div>
        </div>
        <div
          class="register-separator"
          :style="
            progressBarMap[i - 1].isComplete
              ? {
                  background: 'rgba(0, 255, 55, 0.2)',
                }
              : ''
          "
          v-if="item.name == 'separator'"
        ></div>
      </div>
    </div>
    <form class="register-form flex" @submit.prevent="onSubmit()">
      <div style="width: inherit" v-show="page == 1">
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
            v-model="v$.form.personal.surname.$model"
          />
          <div
            class="register-error"
            v-if="
              v$.form.personal.surname.required.$invalid &&
              v$.form.personal.surname.$dirty &&
              !v$.form.personal.surname.alphaRusEng.$invalid
            "
          >
            Заполните, пожалуйста,
            <span style="color: rgba(255, 0, 0, 0.6)">это поле</span>
          </div>

          <div
            class="register-error"
            v-if="
              v$.form.personal.surname.alphaRusEng.$invalid &&
              v$.form.personal.surname.$dirty
            "
          >
            Это <span style="color: rgba(255, 0, 0, 0.6)">поле</span> должно
            содержать только буквы и тире
          </div>
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
            v-model.trim="v$.form.personal.name.$model"
          />
          <div
            class="register-error"
            v-if="
              v$.form.personal.name.required.$invalid &&
              v$.form.personal.name.$dirty &&
              !v$.form.personal.name.alphaRusEng.$invalid
            "
          >
            Заполните, пожалуйста,
            <span style="color: rgba(255, 0, 0, 0.6)">это поле</span>
          </div>

          <div
            class="register-error"
            v-if="
              v$.form.personal.name.alphaRusEng.$invalid &&
              v$.form.personal.name.$dirty
            "
          >
            Это <span style="color: rgba(255, 0, 0, 0.6)">поле</span> должно
            содержать только буквы и тире
          </div>
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
            v-model.trim="v$.form.personal.patronymic.$model"
          />

          <div
            class="register-error"
            v-if="
              v$.form.personal.patronymic.alphaRusEng.$invalid &&
              v$.form.personal.patronymic.$dirty
            "
          >
            Это <span style="color: rgba(255, 0, 0, 0.6)">поле</span> должно
            содержать только буквы и тире
          </div>
        </div>

        <div class="register-formsItem flex">
          <span class="register-labelsName flex a-i-center j-c-flex-end"
            >Выберите пол:</span
          >
          <div>
            <div class="flex">
              <input
                v-model="form.personal.gender"
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
            v-model="v$.form.personal.birthday.$model"
          />
          <div
            class="register-error"
            v-if="
              v$.form.personal.birthday.$error &&
              v$.form.personal.birthday.$dirty
            "
          >
            Заполните, пожалуйста,
            <span style="color: rgba(255, 0, 0, 0.6)">это поле</span>
          </div>
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
            v-model.trim="v$.form.personal.phoneHolder.$model"
            :onchange="correctPhone()"
          />
          <div
            class="register-error"
            v-if="
              v$.form.personal.phoneHolder.required.$invalid &&
              v$.form.personal.phoneHolder.$dirty
            "
          >
            Заполните, пожалуйста,
            <span style="color: rgba(255, 0, 0, 0.6)">это поле</span>
          </div>
          <div
            class="register-error"
            v-if="
              v$.form.personal.phoneHolder.minLength.$invalid &&
              !v$.form.personal.phoneHolder.required.$invalid
            "
          >
            Номер
            <span style="color: rgba(255, 0, 0, 0.6)">телефона </span> должен
            содержать не менее 10 цифр (без +7)
          </div>
          <div
            class="register-error"
            v-if="
              v$.form.personal.phoneHolder.maxLength.$invalid &&
              !v$.form.personal.phoneHolder.required.$invalid
            "
          >
            Номер
            <span style="color: rgba(255, 0, 0, 0.6)">телефона </span> должен
            содержать не более 10 цифр (без +7)
          </div>
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
                v-model="form.personal.clientsGroup.vip"
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
                v-model="form.personal.clientsGroup.problem"
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
                v-model="form.personal.clientsGroup.oms"
              />
              <label for="oms">ОМС</label>
            </div>
          </div>
          <div
            class="register-error"
            v-if="v$.form.personal.clientsGroup.$error"
          >
            Выберите, пожалуйста,
            <span style="color: rgba(255, 0, 0, 0.6)">хотя бы одну</span> группу
            клиентов
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
                v-model="form.personal.doctor"
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
                v-model="form.personal.doctor"
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
                v-model="form.personal.doctor"
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
                value="false"
                v-model="form.personal.sendSms"
              />
              <label for="notSendSms">Нет</label>
            </div>
            <div class="flex">
              <input
                class="register-customRadio"
                type="radio"
                id="sendSms"
                name="smsCheck"
                value="true"
                v-model="form.sendSms"
              />
              <label for="sendSms">Да</label>
            </div>
          </div>
        </div>
      </div>

      <div style="width: inherit" v-if="page == 2">
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
                @click="selectDocumentType(item.textLabel, item.value)"
                >{{ item.textLabel }}</span
              >
            </div>
          </transition>
        </div>

        <div class="register-formsItem flex">
          <label
            class="register-labelsName flex a-i-center j-c-flex-end"
            for="serial"
            >Серия и номер:</label
          >
          <input
            class="register-inputText register-passportSerial"
            type="text"
            id="serial"
            name="passportsSerialNumber"
            placeholder="0000"
            v-model.trim="form.passport.serialNumber.serial"
          />
          <input
            class="register-inputText register-passportNumber"
            type="text"
            id="number"
            name="passportsNumber"
            placeholder="000000"
            v-model.trim="form.passport.serialNumber.number"
          />
        </div>

        <div class="register-formsItem flex">
          <label
            class="register-labelsName flex a-i-center j-c-flex-end"
            for="who"
            >Кем выдан:</label
          >

          <input
            class="register-inputText"
            placeholder="УФМС РФ"
            type="text"
            id="who"
            name="who"
            v-model.trim="form.passport.documentIssuedBy"
          />
        </div>

        <div class="register-formsItem flex">
          <label
            class="register-labelsName flex a-i-center j-c-flex-end"
            for="dateIssue"
            >Дата выдачи:</label
          >

          <input
            class="register-inputText"
            type="date"
            id="dateIssue"
            name="date"
            v-model="v$.form.passport.dateOfIssue.$model"
          />
          <div
            class="register-error"
            v-if="
              v$.form.passport.dateOfIssue.$error &&
              v$.form.passport.dateOfIssue.$dirty
            "
          >
            Заполните, пожалуйста,
            <span style="color: rgba(255, 0, 0, 0.6)">это поле</span>
          </div>
        </div>
      </div>

      <div style="width: inherit" v-if="page == 3">
        <div class="register-formsItem flex">
          <label
            class="register-labelsName flex a-i-center j-c-flex-end"
            for="postIndex"
            >Индекс:</label
          >
          <input
            class="register-inputText"
            placeholder="123456"
            type="text"
            id="postIndex"
            v-model.trim="form.address.index"
          />
        </div>
        <div class="register-formsItem flex">
          <label
            class="register-labelsName flex a-i-center j-c-flex-end"
            for="country"
            >Страна:</label
          >
          <input
            class="register-inputText"
            placeholder="Россия"
            type="text"
            id="country"
            v-model.trim="form.address.country"
          />
        </div>
        <div class="register-formsItem flex">
          <label
            class="register-labelsName flex a-i-center j-c-flex-end"
            for="region"
            >Область:</label
          >
          <input
            class="register-inputText"
            placeholder="Московская область"
            type="text"
            id="region"
            v-model.trim="form.address.region"
          />
        </div>
        <div class="register-formsItem flex">
          <label
            class="register-labelsName flex a-i-center j-c-flex-end"
            for="town"
            >Город:</label
          >
          <input
            class="register-inputText"
            placeholder="Москва"
            type="text"
            id="town"
            v-model.trim="v$.form.address.town.$model"
          />
          <div
            class="register-error"
            v-if="v$.form.address.town.$error && v$.form.address.town.$invalid"
          >
            Заполните, пожалуйста,
            <span style="color: rgba(255, 0, 0, 0.6)">это поле</span>
          </div>
        </div>
        <div class="register-formsItem flex">
          <label
            class="register-labelsName flex a-i-center j-c-flex-end"
            for="street"
            >Улица:</label
          >
          <input
            class="register-inputText"
            placeholder="ул. Стромынка"
            type="text"
            id="street"
            v-model.trim="form.address.street"
          />
        </div>
        <div class="register-formsItem flex">
          <label
            class="register-labelsName flex a-i-center j-c-flex-end"
            for="houseNumber"
            >Дом:</label
          >
          <input
            class="register-inputText"
            placeholder="72"
            type="text"
            id="houseNumber"
            v-model.trim="form.address.houseNumber"
          />
        </div>
      </div>

      <div
        class="register-successful flex j-c-center a-i-center"
        style="width: inherit"
        v-if="page == 4"
      >
        Регистрация успешно завершена! Здравствуйте, {{ form.personal.name }}
        {{ form.personal.patronymic ? form.personal.patronymic : "" }}!<br />
        (в консоли отобразился сформированный объект)
      </div>
    </form>
    <div class="flex" v-if="page != 4">
      <button
        class="register-button"
        type="button"
        v-if="page !== 1"
        @click="validatePage('prev')"
      >
        Назад
      </button>
      <button
        class="register-button"
        type="button"
        @click="validatePage('next')"
      >
        {{ page &lt; 3 ? `Заполнить ${pages[page].name}` : 'Зарегистрироваться' }}
      </button>
    </div>
  </div>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import { required, minLength, maxLength } from "@vuelidate/validators";
export default {
  name: "App",

  data() {
    return {
      page: 1,
      pages: [
        { name: "Личные данные" },
        { name: "Паспортные данные" },
        { name: "Адрес" },
      ],
      isDropdownOpen: false,
      documentTypeSelected: "Паспорт",
      form: {
        personal: {
          name: "",
          surname: "",
          patronymic: "",
          gender: "",
          birthday: "",
          countryCode: "7",
          phoneHolder: "",
          phoneNumber: "",
          clientsGroup: {
            vip: false,
            problem: false,
            oms: false,
          },
          doctor: "",
          sendSms: false,
        },
        passport: {
          documentType: "passport",
          serialNumber: {
            serial: null,
            number: null,
          },
          documentIssuedBy: "",
          dateOfIssue: null,
        },
        address: {
          index: "",
          region: "",
          town: "",
          country: "",
          houseNumber: "",
          street: "",
        },
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
      progressBarMap: [
        { name: "Личные данные", index: 1, isComplete: false },
        { name: "separator" },
        { name: "Паспортные данные", index: 2, isComplete: false },
        { name: "separator" },
        { name: "Адрес", index: 3, isComplete: false },
      ],
    };
  },

  methods: {
    dropdownToggle() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },

    selectDocumentType(documentType, documentValue) {
      this.documentTypeSelected = documentType;
      this.form.documentType = documentValue;
    },

    onSubmit() {
      console.log("this.form", this.form);
    },

    completeBlock(page) {
      if (page == 1) this.progressBarMap[0].isComplete = true;
      if (page == 2) this.progressBarMap[2].isComplete = true;
      if (page == 3) this.progressBarMap[4].isComplete = true;
    },

    validatePage(value) {
      if (this.page == 3 && value == "next") {
        this.v$.form.address.$touch();
        if (!this.v$.form.address.$error) {
          this.completeBlock(this.page);
          this.page++;
          this.onSubmit();
        }
      }

      if (this.page == 2 && value == "next") {
        this.v$.form.passport.$touch();
        if (!this.v$.form.passport.$error) {
          this.completeBlock(this.page);
          this.page++;
        }
      }

      if (this.page == 1 && value == "next") {
        this.v$.form.personal.$touch();
        if (!this.v$.form.personal.$error) {
          this.completeBlock(this.page);
          this.page++;
        }
      }
      if (value == "prev") {
        this.page--;
      }
    },

    correctPhone() {
      this.form.personal.phoneNumber =
        this.form.personal.countryCode + this.form.personal.phoneHolder;
    },

  },

  setup: () => ({
    v$: useVuelidate(),
  }),
  validations() {
    return {
      form: {
        personal: {
          name: {
            required,
            alphaRusEng: (val) => /^(^$)|([-а-яё]+|[-a-z]+)$/i.test(val),
          },
          surname: {
            required,
            alphaRusEng: (val) => /^(^$)|([-а-яё]+|[-a-z]+)$/i.test(val),
          },
          patronymic: {
            alphaRusEng: (val) => /^(^$)|([-а-яё]+|[-a-z]+)$/i.test(val),
          },
          birthday: { required },
          phoneHolder: {
            required,
            minLength: minLength(10),
            maxLength: maxLength(10),
          },
          clientsGroup: {
            requiredIf: function (clientsGroup) {
              return (
                clientsGroup.problem || clientsGroup.vip || clientsGroup.oms
              );
            },
          },
        },
        passport: {
          dateOfIssue: { required },
        },
        address: {
          town: { required },
        },
      },
    };
  },
};
</script>

<style lang="sass">

@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500&display=swap')

html
  font-size: 16px

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
  font-size: 1rem
  margin: 1.250rem auto
  width: 37.5rem
  padding: 0 0 1.250rem 0
  border-radius: 1.188rem
  background: #f2f2f2
  box-shadow:  0.313rem 0.313rem 0.688rem rgba(194, 194, 194, 0.5),0.5rem 0.5rem 0.688rem rgba(255,255,255,0.8)
  border-left: 0.063rem solid rgba(194,194,194, 0.1)
  border-top: 0.063rem solid rgba(194,194,194, 0.1)
  text-shadow: 0.125rem 0.125rem 0.125rem rgba(151, 150, 150, 0.15)

  &-error
    position: absolute
    right: 1.563rem
    top: 0.313rem
    font-size: 0.625rem
    width: 7.5rem
    &-check
      right: 4.375rem

  &-form
    margin: 0 auto 0 0
    width: 100%

  &-title
    margin: 0.625rem 0 1.250rem 0
    color: #BABECC
    font-size: 1.875rem
    font-weight: 600
    text-shadow: 0.063rem 0.063rem 0.063rem #fff


  &-inputText
    font-family: 'Montserrat', sans-serif
    font-weight: 400
    font-size: 0.875rem
    border: none
    padding: 0.625rem 1.250rem
    border-radius: 0.938rem
    background: #f5f5f5
    width: 12.5rem
    box-shadow: inset 0.063rem 0.063rem 0.063rem rgba(208, 208, 208, 0.8), inset -0.063rem -0.063rem 0.063rem rgba(255, 255, 255, 0.8)
    transition: all 0.25s ease-out
    &:focus
      outline: none
      padding: 0.625rem 1.250rem
      border-radius: 0.938rem
      background: #f6f6f6
      box-shadow: inset 0.188rem 0.188rem 0.188rem rgba(208, 208, 208, 0.9), inset -0.188rem -0.188rem 0.188rem rgba(255, 255, 255, 0.9)
    &:hover
      border-radius: 0.938rem
      background: #f6f6f6
      box-shadow: inset 0.188rem 0.188rem 0.188rem rgba(208, 208, 208, 0.9), inset -0.188rem -0.188rem 0.188rem rgba(255, 255, 255, 0.9)
    &::placeholder
      opacity: 0.3
    &::-webkit-inner-spin-button
      -webkit-appearance: none


  &-passportSerial
    width: 2.5rem
    margin: 0 0.625rem 0 0


  &-passportNumber
    width: 6.875rem

  &-formsItem
    width: inherit
    margin: 0.625rem 0
    position: relative

  &-labelsName
    width: 10.625rem
    margin: 0 1.250rem 0 0
    text-shadow: 0.063rem 0.063rem 0 #fff

  &-staticNumber
    font-size: 0.875rem

  &-phoneNumber
    width: 11.25rem
    margin-left: 0.25rem

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
      width: 1.250rem
      height: 1.250rem
      display: inline-flex
      justify-content: center
      align-items: center
      margin: 0.125rem 0.375rem 0.125rem 0
      background: #f5f5f5
      border-radius: 50%
      box-shadow: inset 0.063rem 0.063rem 0.125rem #d3d3d3, inset -0.063rem -0.063rem 0.125rem #ffffff
      transition: all 0.25s ease-out
    &:not(:disabled):not(:checked)+label:hover::before
      box-shadow: inset 0.125rem 0.125rem 0.125rem #d3d3d3, inset -0.125rem -0.125rem 0.125rem #ffffff
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
      width: 1.250rem
      height: 1.250rem
      display: inline-flex
      justify-content: center
      align-items: center
      margin: 0.125rem 0.375rem 0.125rem 0
      background: #f5f5f5
      border-radius: 0.313rem
      box-shadow: inset 0.063rem 0.063rem 0.125rem #d3d3d3, inset -0.063rem -0.063rem 0.125rem #ffffff
      transition: all 0.25s ease-out
    &:not(:disabled):not(:checked)+label:hover::before
      box-shadow: inset 0.125rem 0.125rem 0.125rem #d3d3d3, inset -0.125rem -0.125rem 0.125rem #ffffff
    &:not(:disabled):active+label::before
      background: #d6d6d6
    &:checked+label::before
      content: "✔"
      background: #ffffff
      color: grey

  &-typeDocument
    box-shadow: inset 0.063rem 0.063rem 0.063rem rgba(208, 208, 208, 0.8), inset -0.063rem -0.063rem 0.063rem rgba(255, 255, 255, 0.8)
    font-weight: 400
    border: none
    border-radius: 0.938rem
    padding: 0.625rem 1.250rem
    background: #f5f5f5
    outline: none
    transition: all 1s ease-out
    position: relative
    width: 15rem
    &:after
      content: "▼"
      position: absolute
      right: 5%
    &:focus
      color: transparent
    &:hover
      box-shadow:  0.313rem 0.313rem 0.375rem #d3d3d3, -0.125rem -0.125rem 0.188rem #ffffff
    & span
      font-size: 0.875rem
      text-align: center
      padding: 0.625rem 0.313rem
      margin: 0.125rem 0
      transition: all 0.1s ease-in
      font-weight: 400
      &:hover
        border-radius: 0.938rem
        background: #f5f5f5
        cursor: pointer
        box-shadow: inset 0.188rem 0.188rem 0.188rem rgba(208, 208, 208, 0.9), inset -0.188rem -0.188rem 0.188rem rgba(255, 255, 255, 0.9)


  &-dropdown
    padding: 0.313rem 0.625rem
    position: absolute
    z-index: 1
    right: 0%
    width: 13.75rem
    box-shadow:  0.313rem 0.313rem 0.375rem #d3d3d3, -0.125rem -0.125rem 0.188rem #ffffff
    border-left: 0.063rem solid rgba(194,194,194, 0.1)
    border-top: 0.063rem solid rgba(194,194,194, 0.1)
    font-weight: 300
    &:after
      content: ""
      position: absolute
      right: 5%

  &-progress
    justify-content: space-between
    font-size: 0.75rem
    margin: 0 0 1.250rem 0

  &-circle
    font-weight: 300
    height: 1.875rem
    width: 1.875rem
    background: #f5f5f5
    border-radius: 50%
    box-shadow: inset 0.063rem 0.063rem 0.063rem rgba(208, 208, 208, 0.8), inset -0.063rem -0.063rem 0.063rem rgba(255, 255, 255, 0.8)
    border-left: 0.063rem solid rgba(194,194,194, 0.1)
    border-top: 0.063rem solid rgba(194,194,194, 0.1)

  &-inlineCircle
    background: rgba(0, 255, 55, 0.2)
    display: inline-flex
    justify-content: center
    align-items: center
    border-radius: 50%
    width: 1.563rem
    height: 1.563rem
    &::after
      content: "✓"
      font-size: 1.250rem
      font-weight: 600
      color: #22512a

  &-progressItem
    width: 4.375rem

  &-itemName
    font-weight: 300
    height: 1.875rem

  &-progressItem:nth-child(odd)
    z-index: 2

  &-progressItem:nth-child(even)
    width: 9.875rem
    justify-content: flex-end
    margin: 0 -0.625rem 0 -0.625rem
    z-index: 1

  &-separator
    width: 100%
    height: 0.313rem
    background: #f5f5f5
    margin: 0 0 0.75rem 0
    border-radius: 0.25rem
    box-shadow: inset 0.05rem  0.05rem 0.05rem rgba(208, 208, 208, 0.8), inset -0.063rem -0.063rem 0.05rem rgba(255, 255, 255, 0.8)

  &-button
    border: none
    font-size: 1rem
    font-weight: 300
    border-radius: 0.625rem
    background: #77b3d9
    padding: 0.625rem 1.250rem
    margin: 1.250rem 1.250rem 0.625rem
    border-left: 0.063rem solid rgba(194,194,194, 0.4)
    border-top: 0.063rem solid rgba(194,194,194, 0.4)
    background: #f5f5f5
    box-shadow:  0.188rem 0.188rem 0.5rem #a9a9a9, -0.313rem -0.313rem 0.625rem #ececec
    transition: all 0.1s ease-in
    &:hover
      box-shadow:  0.125rem 0.125rem 0.313rem #a9a9a9, -0.125rem -0.125rem 0.625rem #ececec
      box-shadow:  0.063rem 0.063rem 0.125rem #a9a9a9, -0.063rem -0.063rem 0.125rem #ececec
    &:focus
      outline: none

  &-successful
    height: 9.375rem

.fade-enter-active,
.fade-leave-active
  transition: opacity 0.5s ease-out


.fade-enter-from,
.fade-leave-to
  opacity: 0



@media screen  and (min-height: 600px)
  html
    font-size: 11px

@media screen  and (min-height: 700px)
  html
    font-size: 12px

@media screen  and (min-height: 800px)
  html
    font-size: 14px

@media screen  and (min-height: 900px)
  html
    font-size: 16px

@media screen  and (min-height: 1080px)
  html
    font-size: 18px

</style>
