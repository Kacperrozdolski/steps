<template>
  <line
    @click="removeLine"
    :id="id"
    :x1="position.firstEndpoint.left + 72"
    :y1="position.firstEndpoint.top + 30"
    :x2="position.secondEndpoint.left + 72"
    :y2="position.secondEndpoint.top + 30"
  />
</template>

<script>
export default {
  props: ["position", "id"],
  methods: {
    removeLine($event) {
      let id = $event.path[0].id;
      this.$emit("removeLine", id);
    },
  },
};
</script>

<style scoped>
line {
  cursor: pointer;
  position: absolute;
  background: transparent;
}
</style>