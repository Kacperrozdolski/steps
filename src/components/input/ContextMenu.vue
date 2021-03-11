<template>
  <div
    class="context-menu"
    v-show="show"
    :style="style"
    ref="context"
    tabindex="0"
    @blur="close"
  >
    <li @click="append('parent', $event)">parent</li>
    <li @click="append('child', $event)">child</li>
    <li @click="append('note', $event)">note</li>
    <li @click="append('text', $event)">text</li>
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
    append(value, position) {
      if (value == "parent") {
        this.$emit("parent", position);
        this.close();
      } else if (value == "child") {
        this.$emit("child", position);
        this.close();
      } else if (value == "note") {
        this.$emit("note", position);
        this.close();
      } else {
        this.$emit("text", position);
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