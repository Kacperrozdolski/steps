<template>
  <draggable-div :position="position">
    <textarea
      :id="'element' + id"
      spellcheck="false"
      type="text"
      placeholder="note"
      :value="content"
    ></textarea>
  </draggable-div>
</template>
<script>
import DraggableDiv from "./DraggableDiv.vue";
export default {
  props: ["id", "content", "position"],
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