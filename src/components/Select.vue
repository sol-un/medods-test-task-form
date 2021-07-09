<template>
  <div class="container">
    <label class="form-label" :for="id"
      >{{ `${label}${isRequired ? "*" : ""}` }}
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
    </label>
    <div class="error" v-if="hasError">
      {{ errorMsg }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Select",
  props: {
    id: String,
    label: String,
    data: Object,
    hasError: {
      type: Boolean,
      default: false,
    },
    errorMsg: {
      type: String,
      default: "Содержит ошибку!",
    },
    multiple: {
      type: Boolean,
      default: false,
    },
    firstOptionEmpty: {
      type: Boolean,
      default: false,
    },
    isRequired: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../scss/mixins.scss";
@import "../scss/colors.scss";

.container {
  @include mx(auto);
  position: relative;
  width: fit-content;
}

label {
  @include uppercase;
  display: block;
}

select {
  @include p(5px);
  @include mx(auto);
  @include my(5px);
  display: block;
  width: 100%;

  &:focus-visible {
    outline: none;
  }
}

.error {
  position: absolute;
  color: $imperial-red;
  font-size: 10px;
}
</style>
