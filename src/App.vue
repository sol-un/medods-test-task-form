<template>
  <div id="app">
    <div class="patient-form">
      <h1>Новый клиент</h1>
      <form autocomplete="off" @submit.prevent="submit">
        <fieldset>
          <legend><h2>Общие сведения</h2></legend>
          <div class="form-group text-inputs-group">
            <TextField
              :id="'lastName'"
              :label="'Фамилия'"
              :errors="$v.lastName"
              :value="lastName"
              @input="lastName = $event"
            />
            <TextField
              :id="'firstName'"
              :label="'Имя'"
              :errors="$v.firstName"
              :value="firstName"
              @input="firstName = $event"
            />
            <TextField
              :id="'patronymic'"
              :label="'Отчество'"
              :value="patronymic"
              @input="patronymic = $event"
            />
            <TextField
              :id="'birthDate'"
              :label="'Дата рождения'"
              :errors="$v.birthDate"
              :value="birthDate"
              @input="birthDate = $event"
            />
            <TextField
              :id="'phone'"
              :label="'Номер телефона'"
              :errors="$v.phone"
              :value="phone"
              @input="phone = $event"
            />
            <div class="radio-group">
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
            <Select
              :label="'Группа клиентов'"
              :id="'tags'"
              :errors="$v.tags"
              :data="data.tags"
              :multiple="true"
              v-model="tags"
            />
            <Select
              :label="'Лечащий врач'"
              :id="'attendingDr'"
              :data="data.doctors"
              v-model="attendingDr"
              :firstOptionEmpty="true"
            />
          </div>
          <div class="form-group">
            <Checkbox
              :label="'Не отправлять СМС'"
              :id="'noSms'"
              @change="noSms = $event"
            />
          </div>
        </fieldset>
        <fieldset>
          <legend><h2>Адрес</h2></legend>
          <div class="form-group text-inputs-group last">
            <TextField
              :id="'zipCode'"
              :label="'Индекс'"
              :errors="$v.address.zipCode"
              :value="address.zipCode"
              @input="address.zipCode = $event"
            />
            <TextField
              :id="'country'"
              :label="'Страна'"
              :value="address.country"
              @input="address.country = $event"
            />
            <TextField
              :id="'region'"
              :label="'Область'"
              :value="address.region"
              @input="address.region = $event"
            />
            <TextField
              :id="'city'"
              :label="'Город'"
              :errors="$v.address.city"
              :value="address.city"
              @input="address.city = $event"
            />
            <TextField
              :id="'street'"
              :label="'Улица'"
              :value="address.street"
              @input="address.street = $event"
            />
            <TextField
              :id="'building'"
              :label="'Дом'"
              :value="address.building"
              @input="address.building = $event"
            />
          </div>
        </fieldset>
        <fieldset>
          <legend><h2>Удостоверение личности</h2></legend>
          <Select
            :label="'Тип документа'"
            :id="'type'"
            :errors="$v.id.type"
            :data="data.ids"
            v-model="id.type"
            :firstOptionEmpty="true"
          />
          <div class="form-group text-inputs-group last">
            <TextField
              :id="'issue'"
              :label="'Серия'"
              :errors="$v.id.issue"
              :value="id.issue"
              @input="id.issue = $event"
            />
            <TextField
              :id="'number'"
              :label="'Номер'"
              :errors="$v.id.number"
              :value="id.number"
              @input="id.number = $event"
            />
            <TextField
              :id="'issuer'"
              :label="'Кем выдан'"
              :value="id.issuer"
              @input="id.issuer = $event"
            />
            <TextField
              :id="'issuingDate'"
              :label="'Когда выдан'"
              :value="id.issuingDate"
              :errors="$v.id.issuingDate"
              @input="id.issuingDate = $event"
            />
          </div>
        </fieldset>
        <div class="fine-print">*Поле обязательно для заполнения.</div>
        <Button :text="'Создать'" />
      </form>
    </div>
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
  ids: {
    Паспорт: "Паспорт",
    "Свидетельство о рождении": "Свидетельство о рождении",
    "Вод. удостоверение": "Вод. удостоверение",
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
      address: {
        city: "",
        zipCode: "",
        country: "",
        region: "",
        street: "",
        building: "",
      },
      id: {
        type: "",
        issue: "",
        number: "",
        issuer: "",
        issuingDate: "",
      },
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
      required,
      numeric,
      isFirstCharSeven,
      minLength: minLength(11),
      maxLength: maxLength(11),
    },
    tags: { required },
    address: {
      city: { required },
      zipCode: { numeric },
    },
    id: {
      type: { required },
      issue: { numeric },
      number: { numeric },
      issuingDate: { required },
    },
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
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='4' height='4' viewBox='0 0 4 4'%3E%3Cpath fill='%23a8dadcff' fill-opacity='1' d='M1 3h1v1H1V3zm2-2h1v1H3V1z'%3E%3C/path%3E%3C/svg%3E");
  color: $prussian-blue;
  font-family: "Roboto", sans-serif;
}

fieldset {
  @include border(1px, $prussian-blue);
  @include my(30px);
  border-style: dotted;
}

.patient-form {
  @include mx(auto);
  @include p(25px);
  @include border(2px, $prussian-blue);
  max-width: 800px;
  background: $honeydew;
  filter: drop-shadow(0 16px 32px rgba(0, 0, 0, 0.25));
}

.form-group {
  @include py(20px);
}

.text-inputs-group {
  display: flex;
  flex-wrap: wrap;
  grid-row-gap: 25px;
  grid-column-gap: 25px;
  align-items: center;
}

.radio-group {
  align-self: center;
}

.text-transform-uppercase {
  @include uppercase;
}

.text-align-center {
  text-align: center;
}

.button {
  @include mx(auto);
  @include uppercase;
  width: fit-content;
}

.fine-print {
  @include mb(30px);
  text-align: center;
  font-style: italic;
  font-size: 15px;
}
</style>
