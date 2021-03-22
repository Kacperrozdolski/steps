<template>
  <div ref="draggableContainer" class="draggable-container" :id="id">
    <input
      spellcheck="false"
      type="text"
      :placeholder="placeholder"
      :style="{ background: color.bodyColor, color: color.textColor }"
    />
    <menu id="menu">
      <img
        src="@/assets/ElementMenu/delete.svg"
        @click="deleteElement($event)"
      />
      <img src="@/assets/ElementMenu/palete.svg" />
      <img
        src="@/assets/ElementMenu/connect.svg"
        @click="createConnection($event)"
      />
      <img src="@/assets/ElementMenu/position.svg" @mousedown="dragMouseDown" />
    </menu>
  </div>
</template>

<script>
export default {
  props: ["id", "position", "color", "placeholder"],
  data: function () {
    return {
      selected: false,
      positions: {
        clientX: undefined,
        clientY: undefined,
        movementX: 0,
        movementY: 0,
      },
    };
  },
  mounted() {
    this.$refs.draggableContainer.style.left = this.position.left + "px";
    this.$refs.draggableContainer.style.top = this.position.top + "px";
  },
  methods: {
    createConnection($event) {
      this.selected = !this.selected;
      let id = $event.path[2].id;
      this.$emit("createConnection", id);
    },
    deleteElement($event) {
      let id = $event.path[2].id;
      this.$emit("deleteElement", id);
    },
    dragMouseDown: function (event) {
      event.preventDefault();
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
      this.$refs.draggableContainer.style.top =
        this.$refs.draggableContainer.offsetTop -
        this.positions.movementY +
        "px";
      this.$refs.draggableContainer.style.left =
        this.$refs.draggableContainer.offsetLeft -
        this.positions.movementX +
        "px";
      let left =
        this.$refs.draggableContainer.offsetLeft - this.positions.movementX;
      let top =
        this.$refs.draggableContainer.offsetTop - this.positions.movementY;
      let id = this.$refs.draggableContainer.id;
      this.$emit("changePosition", id, top, left);
    },
    closeDragElement() {
      document.onmouseup = null;
      document.onmousemove = null;
    },
  },
};
</script>

<style scoped>
.draggable-container {
  position: absolute;
}
div:hover > menu {
  top: -35px;
}
input {
  width: 140px;
  height: 60px;
  text-decoration: none !important;
  border: 0;
  padding: 0;
  position: relative;
  z-index: 1;
  text-align: center;
  cursor: pointer;
}

input:focus {
  outline: none;
}
input::placeholder {
  color: black;
}
menu {
  width: 100%;
  height: 35px;
  background: rgb(255, 255, 255);
  position: absolute;
  margin: 0;
  transition: top 0.5s;
  top: 0px;
  padding: 0;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
img {
  height: 23px;
  cursor: pointer;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-drag: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
</style>