<template>
  <div class="form-text-input">
    <label class="sr-only" :for="id">
      {{ placeholder }}
    </label>
    <input
      type="text"
      :class="['text-field', { 'has-error': hasError }]"
      :id="id"
      :placeholder="placeholder"
      :value="value"
      @input="$emit('input', $event.target.value)"
    />
    <div class="error" v-if="hasError">
      {{ `${placeholder} – обязательное поле!` }}
    </div>
  </div>
</template>

<script>
export default {
  name: "TextField",
  props: {
    value: String,
    id: String,
    placeholder: String,
    hasError: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss">
@import "../scss/mixins.scss";
@import "../scss/colors.scss";

.sr-only {
  display: none;
}

.text-field {
  @include p(10px);
  @include border(2px, $prussian-blue);
  width: 100%;
  box-sizing: border-box;

  &:hover {
    @include border(2px, $celadon-blue);
  }

  &:focus-visible {
    outline: none;
    @include border(2px, $celadon-blue);
  }

  &.has-error {
    @include border(2px, $imperial-red);
  }

  &::placeholder {
    font-family: "Roboto", sans-serif;
    text-transform: uppercase;
  }
}

.form-text-input {
  flex: 1 1 350px;
}

.error {
  color: $imperial-red;
}
</style>
