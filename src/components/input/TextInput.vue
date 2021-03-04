<template>
  <draggable-div>
    <textarea
      v-model="text"
      spellcheck="false"
      rows="1"
      placeholder="text"
      :cols="text == '' ? 5 : text.length * 1.8"
    ></textarea>
  </draggable-div>
</template>
<script>
import DraggableDiv from "./DraggableDiv.vue";
export default {
  components: {
    DraggableDiv,
  },
  data() {
    return { text: "" };
  },
};
</script>
<style scoped>
textarea {
  background: transparent;
  color: white;
  outline: none;
  border: none;
  resize: none;
  text-align: center;
  font-size: 30px;
  font-weight: 500;
  padding: 5px;
}
textarea:focus {
  resize: none;
  text-align: center;
  outline: solid 1px rgb(161, 161, 161);
}
textarea::placeholder {
  color: white;
}
</style>