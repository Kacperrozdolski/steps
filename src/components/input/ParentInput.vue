<template>
  <draggable-div><input spellcheck="false" type="text" placeholder="parent" /></draggable-div>
</template>
<script>
import DraggableDiv from "./DraggableDiv.vue";
export default {
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