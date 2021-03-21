<template>
  <div
    class="context-menu"
    v-show="show"
    :style="style"
    ref="context"
    tabindex="0"
    @blur="close"
  >
    <li @click="appendElement('orange', $event)">yellow</li>
    <li @click="appendElement('blue', $event)">blue</li>
    <li @click="appendElement('note', $event)">note</li>
    <li @click="appendElement('text', $event)">text</li>
  </div>
</template>
<script>
export default {
  name: "ContextMenu",
  props: {
    display: Boolean,
  },
  data() {
    return {
      left: 0,
      top: 0,
      show: false,
    };
  },
  computed: {
    style() {
      return {
        top: this.top + "px",
        left: this.left + "px",
      };
    },
  },
  methods: {
    appendElement(value, position) {
      if (value == "orange") {
        this.$emit("appendElement", position, "orange");
        this.close();
      } else if (value == "blue") {
        this.$emit("appendElement", position, "blue");
        this.close();
      } else if (value == "note") {
        this.$emit("appendElement", position, "note");
        this.close();
      }
    },
    close() {
      this.show = false;
      this.left = 0;
      this.top = 0;
    },
    open(evt) {
      this.left = evt.pageX || evt.clientX;
      this.top = evt.pageY || evt.clientY;
      this.$nextTick(() => this.$el.focus());
      this.show = true;
    },
  },
};
</script>
<style>
.contextMenu li {
  width: 120px;
  height: 35px;
  font-weight: 300;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  margin-bottom: 1px;
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none;
}
.contextMenu li:nth-child(1) {
  background: #fadcaa;
}
.contextMenu li:nth-child(2) {
  background: #00c1ff;
}
.contextMenu li:nth-child(3) {
  background: #c0c0c0;
}
.contextMenu li:nth-child(4) {
  background: #ffffff;
}
.context-menu {
  position: absolute;
  z-index: 999;
  outline: none;
  cursor: pointer;
}
</style>