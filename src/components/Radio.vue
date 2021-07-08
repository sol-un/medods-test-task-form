<template>
  <label :for="id" :class="{ checked: isChecked }">
    {{ value }}
    <input
      :name="name"
      type="radio"
      :id="id"
      :option="value"
      @change="$emit('change', $event.target.attributes.option.value)"
    />
  </label>
</template>

<script>
export default {
  name: "Radio",
  model: {
    prop: "option",
    event: "change",
  },
  props: {
    value: String,
    name: String,
    id: String,
    isChecked: Boolean,
  },
};
</script>

<style lang="scss" scoped>
@import "../scss/mixins.scss";
@import "../scss/colors.scss";

label {
  @include p(5px);
  @include uppercase;
  position: relative;
  text-transform: uppercase;

  &:hover {
    color: $celadon-blue;
  }

  &.checked {
    background-color: $prussian-blue;
    color: $honeydew;
  }
}

input {
  position: absolute;
  left: 0;
  opacity: 0;
}
</style>
