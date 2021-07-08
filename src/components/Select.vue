<template>
  <label class="form-label" for="tags"
    >Группа клиентов
    <select id="tags" v-model="tags" multiple>
      <option value="VIP">VIP</option>
      <option value="Проблемные">Проблемные</option>
      <option value="ОМС">ОМС</option>
    </select>
  </label>
</template>

<script>
export default {
  name: "Select",
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
