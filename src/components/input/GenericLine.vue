<template>
  <line
    :x1="position.firstEndpoint.left"
    :y1="position.firstEndpoint.top"
    :x2="position.secondEndpoint.left"
    :y2="position.secondEndpoint.top"
  />
</template>

<script>
export default {
  props: ["position"],
  mounted() {},
};
</script>

<style scoped>
line {
  position: absolute;
  background: transparent;
}
</style>