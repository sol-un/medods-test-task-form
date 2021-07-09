<template>
  <button class="button" type="submit">{{ text }}</button>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: String,
  },
};
</script>

<style lang="scss" scoped>
@import "../scss/mixins.scss";
@import "../scss/colors.scss";

button {
  @include py(10px);
  @include px(15px);
  @include border(1px, $prussian-blue);
  background: $prussian-blue;
  color: $honeydew;
  display: block;

  &:hover {
    background: $celadon-blue;
  }
}
</style>
