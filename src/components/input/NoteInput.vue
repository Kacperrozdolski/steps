<template>
  <draggable-div :position="position">
    <textarea
      :id="id"
      spellcheck="false"
      placeholder="note"
      @input="contentChanged"
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
  methods: {
    contentChanged($event) {
      this.$emit("contentChanged", $event.path[0].id, $event.path[0].value);
    },
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