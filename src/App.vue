<template>
  <div id="app">
    <form @submit.prevent="submit" class="patient-form">
      <div class="form-group text-inputs-group">
        <TextField
          :id="'lastName'"
          :label="'Фамилия'"
          :hasError="$v.lastName.$error"
          :errorMsg="'не может быть пустой!'"
          :value="lastName"
          @input="lastName = $event"
        />
        <TextField
          :id="'firstName'"
          :label="'Имя'"
          :hasError="$v.firstName.$error"
          :errorMsg="'не может быть пустым!'"
          :value="firstName"
          @input="firstName = $event"
        />
        <TextField
          :id="'patronymic'"
          :label="'Отчество'"
          :value="patronymic"
          :hasError="$v.patronymic.$error"
          @input="patronymic = $event"
        />
        <TextField
          :id="'birthDate'"
          :label="'Дата рождения'"
          :hasError="$v.birthDate.$error"
          :errorMsg="'не может быть пустой!'"
          :value="birthDate"
          @input="birthDate = $event"
        />
        <TextField
          :id="'phone'"
          :label="'Номер телефона'"
          :hasError="$v.phone.$error"
          :errorMsg="'не может быть пустым, состоит из 11 цифр и начинается с 7!'"
          :value="phone"
          @input="phone = $event"
        />
      </div>
      <div class="form-group">
        <div class="radio-group">
          <span class="text-transform-uppercase"> Пол: </span>
          <Radio
            :name="'sex'"
            :id="'male'"
            :value="'Муж'"
            :isChecked="sex === 'Муж'"
            @change="sex = $event"
          />
          <Radio
            :name="'sex'"
            :id="'female'"
            :value="'Жен'"
            :isChecked="sex === 'Жен'"
            @change="sex = $event"
          />
        </div>
      </div>
      <div class="form-group">
        <div class="list-group">
          <Select
            :label="'Группа клиентов'"
            :id="'tags'"
            :hasError="$v.tags.$error"
            :errorMsg="'не может быть пустой!'"
            :data="data.tags"
            v-model="tags"
            :multiple="true"
          />
          <Select
            :label="'Лечащий врач'"
            :id="'attendingDr'"
            :data="data.doctors"
            v-model="attendingDr"
            :firstOptionEmpty="true"
          />
        </div>
      </div>
      <div class="form-group">
        <Checkbox
          :label="'Не отправлять СМС'"
          :id="'noSms'"
          @change="noSms = $event"
        />
      </div>
      <Button :text="'Создать'" />
    </form>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import {
  required,
  numeric,
  minLength,
  maxLength,
} from "vuelidate/lib/validators";
import TextField from "./components/TextField.vue";
import Radio from "./components/Radio.vue";
import Select from "./components/Select.vue";
import Checkbox from "./components/Checkbox.vue";
import Button from "./components/Button.vue";

const isFirstCharSeven = (value) => Number(value[0]) === 7;

const data = {
  tags: {
    VIP: "VIP",
    Проблемные: "Проблемные",
    ОМС: "ОМС",
  },
  doctors: {
    Иванов: "Иванов",
    Захаров: "Захаров",
    Чернышева: "Чернышева",
  },
};

export default {
  name: "App",
  data() {
    return {
      data,
      lastName: "",
      firstName: "",
      patronymic: "",
      birthDate: "",
      phone: "",
      sex: "",
      tags: [],
      attendingDr: "",
      noSms: false,
    };
  },
  components: {
    TextField,
    Radio,
    Select,
    Checkbox,
    Button,
  },
  validations: {
    lastName: { required },
    firstName: { required },
    birthDate: { required },
    phone: {
      numeric,
      isFirstCharSeven,
      minLength: minLength(11),
      maxLength: maxLength(11),
    },
    patronymic: {},
    tags: { required },
  },
  methods: {
    submit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        alert("Клиент успешно создан!"); // eslint-disable-line no-alert
      }
    },
  },
  mixins: [validationMixin],
};
</script>

<style lang="scss">
@import "./scss/mixins.scss";
@import "./scss/colors.scss";
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@100;400&display=swap");

body {
  @include py(50px);
  background-color: $honeydew;
  background-image: url("data:image/svg+xml,%3Csvg width='80' height='80' viewBox='0 0 80 80' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23457b9dff' fill-opacity='0.4'%3E%3Cpath d='M50 50c0-5.523 4.477-10 10-10s10 4.477 10 10-4.477 10-10 10c0 5.523-4.477 10-10 10s-10-4.477-10-10 4.477-10 10-10zM10 10c0-5.523 4.477-10 10-10s10 4.477 10 10-4.477 10-10 10c0 5.523-4.477 10-10 10S0 25.523 0 20s4.477-10 10-10zm10 8c4.418 0 8-3.582 8-8s-3.582-8-8-8-8 3.582-8 8 3.582 8 8 8zm40 40c4.418 0 8-3.582 8-8s-3.582-8-8-8-8 3.582-8 8 3.582 8 8 8z' /%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
  color: $prussian-blue;
  font-family: "Roboto", sans-serif;
}

.form-group {
  @include py(25px);
}

.text-inputs-group {
  display: flex;
  flex-wrap: wrap;
  grid-row-gap: 25px;
  grid-column-gap: 25px;
}

.radio-group {
  @include mx(auto);
  width: fit-content;
}

.list-group {
  display: flex;
}

.text-transform-uppercase {
  @include uppercase;
}

.patient-form {
  @include mx(auto);
  @include p(25px);
  @include border(2px, $prussian-blue);
  max-width: 800px;
  background: $honeydew;
}

.button {
  @include mx(auto);
  @include uppercase;
  width: fit-content;
}
</style>
