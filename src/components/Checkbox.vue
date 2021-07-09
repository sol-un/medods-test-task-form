<template>
  <div class="container">
    <label class="form-label" :for="id"
      >{{ label }}
      <input
        type="checkbox"
        :id="id"
        :checked="isChecked"
        @change="$emit('change', Boolean($event.target.value))"
      />
    </label>
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

.container {
  @include mx(auto);
  width: fit-content;
}

input {
  @include my(0);
  @include ml(10px);
  vertical-align: middle;
}
</style>
