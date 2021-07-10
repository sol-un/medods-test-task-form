<template>
  <div id="app">
    <div class="patient-form">
      <h1>Новый клиент</h1>
      <form @submit.prevent="submit">
        <fieldset>
          <legend><h2>Общие сведения</h2></legend>
          <div class="form-group text-inputs-group">
            <TextField
              :id="'lastName'"
              :label="'Фамилия'"
              :hasError="$v.lastName.$error"
              :errorMsg="messages.required"
              :value="lastName"
              :isRequired="true"
              @input="lastName = $event"
            />
            <TextField
              :id="'firstName'"
              :label="'Имя'"
              :hasError="$v.firstName.$error"
              :errorMsg="messages.required"
              :value="firstName"
              :isRequired="true"
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
              :hasError="$v.birthDate.$error"
              :errorMsg="messages.required"
              :value="birthDate"
              :isRequired="true"
              @input="birthDate = $event"
            />
            <TextField
              :id="'phone'"
              :label="'Номер телефона'"
              :hasError="$v.phone.$error"
              :errorMsg="messages.phone"
              :value="phone"
              :isRequired="true"
              @input="phone = $event"
            />
          </div>
          <div class="form-group text-align-center">
            <span class="text-transform-uppercase">Пол</span>
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
            <div class="list-group">
              <Select
                :label="'Группа клиентов'"
                :id="'tags'"
                :hasError="$v.tags.$error"
                :errorMsg="messages.required"
                :data="data.tags"
                :isRequired="true"
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
          </div>
          <div class="form-group last">
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
              :hasError="$v.address.city.$error"
              :errorMsg="messages.required"
              :value="address.city"
              :isRequired="true"
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
            :id="'id'"
            :data="data.ids"
            :isRequired="true"
            v-model="id.type"
          />
          <div class="form-group text-inputs-group last">
            <TextField
              :id="'issue'"
              :label="'Серия'"
              :value="id.issue"
              @input="id.issue = $event"
            />
            <TextField
              :id="'number'"
              :label="'Номер'"
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
              :hasError="$v.id.issuingDate.$error"
              :errorMsg="messages.required"
              :isRequired="true"
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

const messages = {
  required: "Не может быть пустым!",
  phone: "Не может быть пустым, состоит из 11 цифр и начинается с 7!",
};

export default {
  name: "App",
  data() {
    return {
      data,
      messages,
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
      numeric,
      isFirstCharSeven,
      minLength: minLength(11),
      maxLength: maxLength(11),
    },
    tags: { required },
    address: {
      city: { required },
    },
    id: {
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
  border-bottom: 1px dotted $prussian-blue;

  &.last {
    border-bottom: 0;
  }
}

.text-inputs-group {
  display: flex;
  flex-wrap: wrap;
  grid-row-gap: 25px;
  grid-column-gap: 25px;
}

.radio-group {
  @include mx(auto);
  @include mt(15px);
  width: fit-content;
}

.list-group {
  display: flex;
  flex-wrap: wrap;
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
