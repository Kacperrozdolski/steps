<template>
  <div
    ref="draggableContainer"
    @mouseleave="paleteLeave"
    class="draggable-container"
    :id="id"
  >
    <input
      @focus="inputFocused"
      @blur="inputBlur"
      class="genericElement"
      spellcheck="false"
      type="text"
      @input="changeContenet"
      :placeholder="content == '' ? placeholder : content"
      :value="content"
      :style="{
        background: color.bodyColor,
        color: color.textColor,
      }"
      style="--color:{{color.bodyColor}}"
    />
    <menu id="menu">
      <div v-if="palete" class="palete">
        <div class="colorInput">
          <div :style="{ background: color.bodyColor }" class="textColor"></div>
          <input
            type="text"
            :placeholder="color.bodyColor"
            v-on:keyup.enter="changeBodycolor"
          />
        </div>
        <div class="colorInput">
          <div :style="{ background: color.textColor }" class="textColor"></div>
          <input
            type="text"
            :placeholder="color.textColor"
            v-on:keyup.enter="changeTextcolor"
          />
        </div>
      </div>
      <img
        src="@/assets/ElementMenu/delete.svg"
        @click="deleteElement($event)"
      />
      <img src="@/assets/ElementMenu/palete.svg" @click="togglePalete()" />
      <img
        src="@/assets/ElementMenu/connect.svg"
        @click="createConnection($event)"
      />
      <img
        src="@/assets/ElementMenu/position.svg"
        @mousedown="dragMouseDown"
        @mouseover="paleteLeave"
      />
    </menu>
  </div>
</template>

<script>
export default {
  props: ["id", "position", "color", "placeholder", "content"],
  data() {
    return {
      selected: false,
      palete: false,
      positions: {
        clientX: undefined,
        clientY: undefined,
        movementX: 0,
        movementY: 0,
        isActive: false,
      },
    };
  },
  mounted() {
    this.$refs.draggableContainer.style.left = this.position.left + "px";
    this.$refs.draggableContainer.style.top = this.position.top + "px";
  },
  methods: {
    changeContenet($event) {
      let id = $event.path[1].id;
      let content = $event.path[0].value;
      this.$emit("changeContent", id, content);
    },
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
    togglePalete() {
      this.palete = !this.palete;
    },
    paleteLeave() {
      this.palete = false;
    },
    changeBodycolor(value) {
      let id;
      let color;
      id = value.path[4].id;
      color = value.path[0].value.toUpperCase();
      if (/^#([0-9A-F]{3}){1,2}$/.test(color)) {
        console.log("A TO JEST HEX WALUE I GITEZ");
        this.$emit("changeBodycolor", id, color);
      } else {
        console.log("TO NIE JE HEX WALUE");
      }
    },
    changeTextcolor(value) {
      let id;
      let color;
      id = value.path[4].id;
      color = value.path[0].value.toUpperCase();
      if (/^#([0-9A-F]{3}){1,2}$/.test(color)) {
        console.log("A TO JEST HEX WALUE I GITEZ");
        this.$emit("changeTextcolor", id, color);
      } else {
        console.log("TO NIE JE HEX WALUE");
      }
    },
    inputFocused($event) {
      let id = $event.path[1].id;
      this.$emit("stackHighest",id)
    },
    inputBlur($event) {
      let id = $event.path[1].id;
      this.$emit("stackNormaly",id)
    },
  },
};
</script>

<style scoped>
.floating {
  animation-name: floating;
  animation-duration: 1s;
  animation-iteration-count: infinite;
  animation-timing-function: ease-in-out;
}

@keyframes floating {
  0% {
    transform: translate(0, 0px);
  }
  50% {
    transform: translate(0, 15px);
  }
  100% {
    transform: translate(0, 0px);
  }
}
.draggable-container {
  position: absolute;
}
div:hover > menu {
  top: -35px;
}
.genericElement {
  width: 140px;
  height: 60px;
  text-decoration: none !important;
  border: 0;
  outline: 0;
  padding: 0;
  position: relative;
  z-index: 1;
  text-align: center;
  cursor: pointer;
}

input:focus {
  outline: none;
}
.genericElement::placeholder {
  color: var(--color);
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
.palete {
  position: absolute;
  width: 100%;
  height: 60px;
  bottom: 100%;
  margin: 0;
  background: #ffffff;
}
.colorInput {
  width: 100%;
  height: 50%;
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
}
.colorInput input {
  width: 60%;
  height: 23px;
  margin: 0;
  background: #dfdfdf;
  border: none;
}
.textColor {
  height: 25px;
  width: 25px;
}
.highestIndex {
  z-index: 99;
}
</style>