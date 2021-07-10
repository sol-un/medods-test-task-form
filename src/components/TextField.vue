<template>
  <div class="form-text-input">
    <label class="sr-only" :for="id">
      {{ label }}
    </label>
    <input
      type="text"
      :class="[
        'text-field',
        { 'has-error': errors && errors.$invalid && errors.$dirty },
      ]"
      :id="id"
      :placeholder="`${label}${this.isRequired ? '*' : ''}`"
      :value="value"
      @input="$emit('input', $event.target.value)"
    />
    <div class="error" v-if="errors && errors.$invalid && errors.$dirty">
      {{ formatErrorMsg(label, errors) }}
    </div>
  </div>
</template>

<script>
export default {
  name: "TextField",
  data() {
    return {
      isRequired: this.errors?.required !== undefined,
      messages: {
        required: "обязательно для заполнения",
        numeric: "состоит из цифр",
        minLength: `содержит не менее ${this.errors?.$params.minLength?.min} символов`,
        maxLength: `содержит не более ${this.errors?.$params.maxLength?.max} символов`,
        isFirstCharSeven: "начинается с 7",
      },
    };
  },
  methods: {
    formatErrorMsg(fieldName, errors) {
      const errorMsgs = Object.keys(errors)
        .filter((key) => key[0] !== "$" && !errors[key])
        .map((key) => this.messages[key]);
      return `Поле "${fieldName}" ${errorMsgs.join(", ")}!`;
    },
  },
  props: {
    value: String,
    id: String,
    label: String,
    errors: Object,
  },
};
</script>

<style lang="scss" scoped>
@import "../scss/mixins.scss";
@import "../scss/colors.scss";

.sr-only {
  display: none;
}

.text-field {
  @include p(10px);
  @include border(1px, $prussian-blue);
  width: 100%;
  box-sizing: border-box;

  &:hover {
    @include border(1px, $celadon-blue);

    &::placeholder {
      color: $celadon-blue;
    }
  }

  &:focus-visible {
    outline: none;

    &::placeholder {
      text-align: right;
    }
  }

  &.has-error {
    @include border(1px, $imperial-red);
  }

  &::placeholder {
    font-family: "Roboto", sans-serif;
    text-transform: uppercase;
    color: $prussian-blue;
  }
}

.form-text-input {
  position: relative;
  flex: 1 1 350px;
}

.error {
  @include ml(10px);
  position: absolute;
  color: $imperial-red;
  font-size: 10px;
}
</style>
