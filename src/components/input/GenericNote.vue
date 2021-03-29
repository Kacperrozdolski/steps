<template>
  <div
    ref="draggableContainer"
    @mouseover="toggleHover"
    @mouseleave="untoggleHover"
    class="draggable-container"
    :id="id"
  >
    <textarea
      class="genericNote"
      spellcheck="false"
      @focus="inputFocused"
      @blur="inputBlur"
      @input="changeContenet"
      :placeholder="content == '' ? 'note' : content"
      :value="content"
      :style="{
        background: color.bodyColor,
        color: color.textColor,
        resize: hover ? 'both' : 'none',
      }"
      style="--color:{{color.bodyColor}}"
    />
    <menu id="menu" @mouseover="toggleHover">
      <div v-if="palete" class="palete">
        <div class="colorInput">
          <div :style="{ background: color.bodyColor }" class="textColor"></div>
          <input
            type="text"
            :placeholder="color.bodyColor"
            @keyup.enter="changeBodycolor"
          />
        </div>
        <div class="colorInput">
          <div :style="{ background: color.textColor }" class="textColor"></div>
          <input
            type="text"
            :placeholder="color.textColor"
            @keyup.enter="changeTextcolor"
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
        @mouseover="untoggleHover"
      />
    </menu>
  </div>
</template>

<script>
export default {
  props: ["id", "position", "color", "placeholder", "content"],
  data() {
    return {
      hover: false,
      selected: false,
      palete: false,
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
    untoggleHover() {
      this.hover = false;
      this.palete = false;
    },
    toggleHover() {
      this.hover = true;
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
    changeContenet($event) {
      let id = $event.path[1].id;
      let content = $event.path[0].value;
      this.$emit("changeContent", id, content);
    },
    inputFocused($event) {
      console.log($event)
      let id = $event.path[1].id;
      this.$emit("stackHighest", id);
    },
    inputBlur($event) {
      let id = $event.path[1].id;
      this.$emit("stackNormaly", id);
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
    transform: translate(0, 5px);
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
.genericNote {
  min-width: 180px;
  min-height: 90px;
  border: 0;
  padding: 0;
  text-decoration: none !important;
  position: relative;
  z-index: 1;
  text-align: center;
  cursor: pointer;
}

.genericNote:focus {
  outline: none;
}
.genericNote::placeholder {
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