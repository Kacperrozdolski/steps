<template>
  <draggable-div :position="position">
    <input
      spellcheck="false"
      type="text"
      placeholder="child"
      @input="contentChanged"
      :id="id"
      :value="content"
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
  background: #c0ddff;
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