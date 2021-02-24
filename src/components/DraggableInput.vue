<template>
  <div
    @mousedown="drag"
    ref="div"
    :positionY="positionY"
    :positionX="positionX"
    :style="{ left: `${positionX}px`, top: `${positionY}px` }"
  >
    <input
      class="family"
      v-if="placeholder == 'parent' || placeholder == 'child'"
      type="text"
      :placeholder="placeholder"
      :background="background"
      :style="{ 'background-color': background }"
      spellcheck="false"
    />
    <textarea
      :placeholder="placeholder"
      v-if="placeholder == 'note'"
      cols="38"
      rows="3"
      spellcheck="false"
    ></textarea>
    <div class="asd" :placeholder="placeholder" v-if="placeholder == 'text'">
      <input
        class="text"
        spellcheck="false"
        :background="background"
        :placeholder="placeholder"
        type="text"
      />
    </div>
  </div>
</template>
  
<script>
export default {
  name: "DraggableInput",
  props: {
    placeholder: {
      type: String,
      required: true,
    },
    background: {
      type: String,
      default: "#fff",
    },
    positionX: {
      type: Number,
      default: 0,
    },
    positionY: {
      type: Number,
      default: 0,
    },
  },
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
    drag(e) {
      this.positions.clientX = e.clientXs;
      this.positions.clientY = e.clientY;
      document.onmousemove = this.elementDrag;
      document.onmouseup = this.closeDragElement;
    },
    elementDrag: function (event) {
      event.preventDefault();
      this.positions.movementX = this.positions.clientX - event.clientX;
      this.positions.movementY = this.positions.clientY - event.clientY;
      this.positions.clientX = event.clientX;
      this.positions.clientY = event.clientY;
      this.$refs.div.style.top =
        this.$refs.div.offsetTop - this.positions.movementY + "px";
      this.$refs.div.style.left =
        this.$refs.div.offsetLeft - this.positions.movementX + "px";
    },
    closeDragElement() {
      document.onmouseup = null;
      document.onmousemove = null;
    },
  },
};
</script>

<style scoped>
div {
  width: 120px;
  height: 40px;
  background: transparent;
  position: absolute;
  border-radius: 0.1rem;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
}
.family {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  border: none;
  outline: none;
  border-radius: 0.1rem;
  padding: 10px;
  overflow: hidden;
}
.text {
  background: transparent;
  color: white;
  border: none;
  outline: none;
  text-align: center;
  font-size: 45px;
  width: 100%;
}
.text:focus {
  border: none;
  outline: none;
}
textarea {
  position: relative;
  text-align: left;
  outline: none;
  border-radius: 0.1rem;
  padding: 10px;
}
::-webkit-input-placeholder {
  text-align: center;
}
:-moz-placeholder {
  text-align: center;
}
</style>
