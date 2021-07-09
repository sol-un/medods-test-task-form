<template>
  <div class="form-text-input">
    <label class="sr-only" :for="id">
      {{ label }}
    </label>
    <input
      type="text"
      :class="['text-field', { 'has-error': hasError }]"
      :id="id"
      :placeholder="label"
      :value="value"
      @input="$emit('input', $event.target.value)"
    />
    <div class="error" v-if="hasError">
      {{ `${label} ${errorMsg}` }}
    </div>
  </div>
</template>

<script>
export default {
  name: "TextField",
  props: {
    value: String,
    id: String,
    label: String,
    errorMsg: {
      type: String,
      default: "содержит ошибку!",
    },
    hasError: {
      type: Boolean,
      default: false,
    },
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
      color: white;
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
  position: absolute;
  left: 10px;
  color: $imperial-red;
  font-size: 10px;
}
</style>
