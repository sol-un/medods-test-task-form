<template>
  <div id="app">
    <section class="form-section">
      <form @submit.prevent="submit" class="patient-form">
        <div class="form-text-inputs">
          <TextField
            :id="'lastName'"
            :placeholder="'Фамилия'"
            :v="$v.lastName"
            v-model="lastName"
          />
          <TextField
            :id="'firstName'"
            :placeholder="'Имя'"
            :v="$v.firstName"
            v-model="firstName"
          />
          <TextField
            :id="'patronymic'"
            :placeholder="'Отчество'"
            :v="$v.patronymic"
            v-model="patronymic"
          />
          <TextField
            :id="'birthDate'"
            :placeholder="'Дата рождения'"
            :v="$v.birthDate"
            v-model="birthDate"
          />
          <TextField
            :id="'phone'"
            :placeholder="'Номер телефона'"
            :v="$v.phone"
            v-model="phone"
          />
        </div>
        <fieldset>
          <legend>Пол</legend>
          <label>
            Жен.
            <input
              name="sex"
              value="Жен."
              type="radio"
              id="female"
              v-model="sex"
            />
          </label>
          <label>
            Муж.
            <input
              name="sex"
              value="Муж."
              type="radio"
              id="male"
              v-model="sex"
            />
          </label>
        </fieldset>
        <label class="form-label" for="tags">Группа клиентов</label>
        <select id="tags" v-model="tags" multiple>
          <option value="VIP">VIP</option>
          <option value="Проблемные">Проблемные</option>
          <option value="ОМС">ОМС</option>
        </select>
        <label class="form-label" for="attendingDr">Лечащий врач</label>
        <select id="attendingDr" v-model="attendingDr">
          <option value="Иванов">Иванов</option>
          <option value="Захаров">Захаров</option>
          <option value="Чернышева">Чернышева</option>
        </select>
        <label class="form-label" for="noSms">Не отправлять СМС</label
        ><input type="checkbox" id="noSms" v-model="noSms" />
        <button class="button" type="submit">Создать</button>
      </form>
    </section>
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

const isFirstCharSeven = (value) => Number(value[0]) === 7;

export default {
  name: "App",
  data() {
    return {
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

fieldset {
  border: 0;
  @include m(0);
  @include p(0);
}

.patient-form {
  @include mx(auto);
  @include p(25px);
  @include border(2px, $prussian-blue);
  max-width: 800px;
  background: $honeydew;
}

.form-text-inputs {
  display: flex;
  flex-wrap: wrap;
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}

.button {
  @include mx(auto);
  width: fit-content;
  text-transform: uppercase;
}
</style>
