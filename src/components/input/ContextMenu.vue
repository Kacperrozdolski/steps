<template>
  <div
    class="context-menu"
    v-show="show"
    :style="style"
    ref="context"
    tabindex="0"
    @blur="close"
  >
    <li @click="append('parent')">parent</li>
    <li @click="append('child')">child</li>
    <li @click="append('note')">note</li>
    <li @click="append('text')">text</li>
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
    append(value) {
      if (value == "parent") {
        this.$emit("parent");
        this.close();
      } else if (value == "child") {
        this.$emit("child");
        this.close();
      } else if (value == "note") {
        this.$emit("note");
        this.close();
      } else {
        this.$emit("text");
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
.context-menu {
  position: absolute;
  z-index: 999;
  outline: none;
  cursor: pointer;
}
</style>