<template>
  <div>
    <label class="form-label" :for="id"
      >{{ `${label}${this.isRequired ? "*" : ""}` }}
    </label>
    <select
      :id="id"
      :multiple="multiple"
      @input="$emit('input', $event.target.value)"
    >
      <option value="" v-if="firstOptionEmpty"></option>
      <option v-for="(value, key) in data" :key="key" :value="key">
        {{ value }}
      </option>
    </select>
    <span class="error" v-if="errors && errors.$invalid && errors.$dirty">
      {{ formatErrorMsg(label, errors) }}
    </span>
  </div>
</template>

<script>
export default {
  name: "Select",
  data() {
    return {
      isRequired: this.errors?.required !== undefined,
      messages: {
        required: "обязательно для заполнения",
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
    id: String,
    label: String,
    data: Object,
    errors: Object,
    multiple: {
      type: Boolean,
      default: false,
    },
    firstOptionEmpty: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../scss/mixins.scss";
@import "../scss/colors.scss";

label {
  @include mt(10px);
  @include mb(5px);
  @include uppercase;
  display: block;
}

select {
  @include p(5px);
  @include mx(auto);
  display: block;
  width: 100%;

  &:focus-visible {
    outline: none;
  }
}

.error {
  color: $imperial-red;
  font-size: 10px;
}
</style>
