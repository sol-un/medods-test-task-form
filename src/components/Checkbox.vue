<template>
  <div class="container">
    <label class="form-label" :for="id">{{ label }} </label>
    <input
      type="checkbox"
      :id="id"
      :checked="isChecked"
      @change="$emit('change', Boolean($event.target.value))"
    />
  </div>
</template>

<script>
export default {
  name: "Checkbox",
  model: {
    prop: "checked",
    event: "change",
  },
  props: {
    id: String,
    label: String,
    isChecked: {
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
  vertical-align: middle;
  line-height: 1;

  &:hover {
    color: $celadon-blue;
  }
}

input {
  @include my(0);
  @include ml(10px);
  vertical-align: middle;
}

.container {
  @include mx(auto);
  width: fit-content;
}
</style>
