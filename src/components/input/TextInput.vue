<template>
  <draggable-div :position="position">
    <input
      spellcheck="false"
      :id="id"
      placeholder="text"
      :value="content"
      @input="contentChanged"
      @click="console"
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
  methods: {
    contentChanged($event) {
      this.$emit("contentChanged", $event.path[0].id, $event.path[0].value);
    },
  },
};
</script>
<style scoped>
input {
  background: transparent;
  color: white;
  outline: none;
  border: none;
  text-align: center;
  font-size: 30px;
  font-weight: 500;
  padding: 5px;
  width: 8.5ch;
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