<template>
  <div id="app">
    <section class="form-section">
      <div class="patient-form">
        <form>
          <div class="form-input">
            <label class="form-label sr-only" for="lastName">Фамилия</label
            ><input
              type="text"
              class="text-field"
              id="lastName"
              placeholder="Фамилия"
              v-model="lastName"
            />
          </div>
          <div class="form-input">
            <label class="form-label sr-only" for="firstName">Имя</label
            ><input
              type="text"
              class="text-field"
              id="firstName"
              placeholder="Имя"
              v-model="firstName"
            />
          </div>
          <div class="form-input">
            <label class="form-label sr-only" for="patronymic">Отчество</label
            ><input
              type="text"
              class="text-field"
              id="patronymic"
              placeholder="Отчество"
              v-model="patronymic"
            />
          </div>
          <div class="form-input">
            <label class="form-label sr-only" for="birthDate"
              >Дата рождения</label
            ><input
              type="text"
              class="text-field"
              id="birthDate"
              placeholder="Дата рождения"
              v-model="birthDate"
            />
          </div>
          <div class="form-input">
            <label class="form-label sr-only" for="phone">Номер телефона</label
            ><input
              type="text"
              class="text-field"
              id="phone"
              placeholder="Номер телефона"
              v-model="phone"
            />
            <div class="error" v-if="$v.phone.$error">Field B is required.</div>
          </div>
          <div class="form-input">
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
          </div>
          <div class="form-input">
            <label class="form-label" for="tags">Группа клиентов</label>
            <select id="tags" v-model="tags" multiple>
              <option value="VIP">VIP</option>
              <option value="Проблемные">Проблемные</option>
              <option value="ОМС">ОМС</option>
            </select>
          </div>
          <div class="form-input">
            <label class="form-label" for="attendingDr">Лечащий врач</label>
            <select id="attendingDr" v-model="attendingDr">
              <option value="Иванов">Иванов</option>
              <option value="Захаров">Захаров</option>
              <option value="Чернышева">Чернышева</option>
            </select>
          </div>
          <div class="form-input">
            <label class="form-label" for="noSms">Не отправлять СМС</label
            ><input type="checkbox" id="noSms" v-model="noSms" />
          </div>
          <button @click="submit">Создать</button>
        </form>
      </div>
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
    submit(e) {
      e.preventDefault();
      this.$v.$touch();
      if (!this.$v.$invalid) {
        alert("Клиент успешно создан!"); // eslint-disable-line no-alert
      }
      console.log(this.$v);
    },
  },
  mixins: [validationMixin],
};
</script>

<style lang="scss">
@import "./scss/mixins.scss";

$imperial-red: #e63946ff;
$honeydew: #f1faeeff;
$powder-blue: #a8dadcff;
$celadon-blue: #457b9dff;
$prussian-blue: #1d3557ff;

body {
  background-color: $honeydew;
  background-image: url("data:image/svg+xml,%3Csvg width='80' height='80' viewBox='0 0 80 80' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23457b9dff' fill-opacity='0.4'%3E%3Cpath d='M50 50c0-5.523 4.477-10 10-10s10 4.477 10 10-4.477 10-10 10c0 5.523-4.477 10-10 10s-10-4.477-10-10 4.477-10 10-10zM10 10c0-5.523 4.477-10 10-10s10 4.477 10 10-4.477 10-10 10c0 5.523-4.477 10-10 10S0 25.523 0 20s4.477-10 10-10zm10 8c4.418 0 8-3.582 8-8s-3.582-8-8-8-8 3.582-8 8 3.582 8 8 8zm40 40c4.418 0 8-3.582 8-8s-3.582-8-8-8-8 3.582-8 8 3.582 8 8 8z' /%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
}

fieldset {
  border: 0;
  @include m(0);
  @include p(0);
}

.sr-only {
  display: none;
}

.patient-form {
  @include my(auto);
  @include border(2px, $prussian-blue);
  display: flex;
  flex-wrap: wrap;
  background: $honeydew;
  max-width: 1110px;
}

.form-input {
  @include mt(10px);
  @include mb(10px);
  @include p(15px);
}

.form-label {
  @include mr(10px);
}

.text-field {
  @include p(10px);
  @include border(2px, $prussian-blue);

  &:focus-visible {
    outline: none;
    @include border(2px, $celadon-blue);
  }

  &:hover {
    @include border(2px, $celadon-blue);
  }
}

.text-field::placeholder {
  text-transform: uppercase;
}
</style>
