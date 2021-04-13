<template>
  <div class="form-control">
    <label :for="id">{{ label }}</label>
    <textarea :id="id" :value="value" @input="change" rows="3"></textarea>
  </div>
</template>

<script>
export default {
  emits: ["update:value"],
  props: {
    label: String,
    value: String,
  },
  data() {
    return {
      id: "input-" + Math.random(),
    };
  },
  methods: {
    change(event) {
      this.$emit("update:value", event.target.value);
    },
  },
};
</script>

<style lang="scss" scoped></style>
