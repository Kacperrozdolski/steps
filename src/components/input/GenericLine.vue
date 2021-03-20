<template>
  <line
    :x1="position.firstEndpoint.left + 72"
    :y1="position.firstEndpoint.top + 30"
    :x2="position.secondEndpoint.left + 72"
    :y2="position.secondEndpoint.top + 30"
  />
</template>

<script>
export default {
  props: ["position"],
};
</script>

<style scoped>
line {
  position: absolute;
  background: transparent;
}
</style>