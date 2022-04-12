<template>
  <input
    class="border-2"
    @input="$emit('update:modelValue', $event.target.value)"
    :value="modelValue"
  />
</template>

<script>
export default {
  name: "SearchBar",
  props: ["modelValue"],
  emits: ["update:modelValue"],
  methods: {
    handleInput(event) {
      this.$emit("update:modelValue", event.target.value);
      console.log(event.target.value);
    },
  },
};
</script>
