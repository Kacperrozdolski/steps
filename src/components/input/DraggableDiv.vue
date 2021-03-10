<template>
  <div
    ref="draggableContainer"
    id="draggable-container"
    @mousedown.shift="dragMouseDown"
    :style="{ left: position.positionX + 'px', top: position.positionY + 'px' }"
  >
    <slot></slot>
  </div>
</template>
  
<script>
export default {
  name: "DraggableDiv",
  props: ["position"],
  data: function () {
    return {
      positions: {
        clientX: undefined,
        clientY: undefined,
        movementX: 0,
        movementY: 0,
      },
    };
  },
  methods: {
    dragMouseDown: function (event) {
      event.preventDefault();
      // get the mouse cursor position at startup:
      this.positions.clientX = event.clientX;
      this.positions.clientY = event.clientY;
      document.onmousemove = this.elementDrag;
      document.onmouseup = this.closeDragElement;
    },
    elementDrag: function (event) {
      event.preventDefault();
      this.positions.movementX = this.positions.clientX - event.clientX;
      this.positions.movementY = this.positions.clientY - event.clientY;
      this.positions.clientX = event.clientX;
      this.positions.clientY = event.clientY;
      // set the element's new position:
      this.$refs.draggableContainer.style.top =
        this.$refs.draggableContainer.offsetTop -
        this.positions.movementY +
        "px";
      this.$refs.draggableContainer.style.left =
        this.$refs.draggableContainer.offsetLeft -
        this.positions.movementX +
        "px";
    },
    closeDragElement($event) {
      document.onmouseup = null;
      document.onmousemove = null;
      let positionX =
        this.$refs.draggableContainer.offsetLeft - this.positions.movementX;
      let positionY =
        this.$refs.draggableContainer.offsetTop - this.positions.movementY;
      let id = $event.path[0].id;
      this.$emit("positionChanged", id, positionX, positionY);
    },
  },
};
</script>

<style scoped>
#draggable-container {
  position: absolute;
  left: 0%;
  top: 0%;
  z-index: 9;
}
</style>
