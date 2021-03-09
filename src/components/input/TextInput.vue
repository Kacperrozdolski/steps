<template>
  <draggable-div :position="position">
    <input
      spellcheck="false"
      role="textbox"
      contenteditable="true"
      placeholder="text"
    />
  </draggable-div>
</template>
<script>
import DraggableDiv from "./DraggableDiv.vue";
export default {
  props: ["id", "content", "position"],
  components: {
    DraggableDiv,
  },
  data() {
    return { text: "" };
  },
};
</script>
<style scoped>
input {
  background: transparent;
  color: white;
  outline: none;
  border: none;
  resize: none;
  text-align: center;
  font-size: 30px;
  min-width: 10%;
  font-weight: 500;
  padding: 5px;
  width: 100%;
}
input:focus {
  resize: none;
  text-align: center;
  outline: solid 1px rgb(161, 161, 161);
}
input::placeholder {
  color: white;
}
</style>