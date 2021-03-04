<template>
  <draggable-div>
    <textarea spellcheck="false" placeholder="note"></textarea>
  </draggable-div>
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
textarea {
  width: 230px;
  height: 100px;
  overflow: auto;
  padding: 10px;
}

textarea:focus {
  outline: none;
}
textarea::placeholder {
  color: black;
  text-align: center;
}
</style>