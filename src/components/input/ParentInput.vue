<template>
  <draggable-div :top="top" :left="left"
    ><input
      spellcheck="false"
      type="text"
      placeholder="parent"
      :value="content"
  /></draggable-div>
</template>
<script>
import DraggableDiv from "./DraggableDiv.vue";
export default {
  props: ["id", "content","top","left"],
  components: {
    DraggableDiv,
  },
};
</script>
<style scoped>
input {
  background: #ffc88a;
  height: 60px;
  border-radius: 0.15rem;
  width: 120px;
  border: none;
  text-align: center;
}
input:focus {
  outline: none;
}
input::placeholder {
  color: black;
}
</style>