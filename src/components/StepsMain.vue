<template>
  <div class="steps-landing">
    <img src="@/assets/logo.svg" @click="console" class="logo" />
    <div @click.ctrl="openContextMenu" class="container" ref="container">
      <component
        v-for="items in elements"
        :key="items.id"
        :is="items.type"
        :placeholder="items.placeholder"
        :background="items.background"
        :positionY="items.positionY"
        :positionX="items.positionX"
      />
    </div>
    <context-menu class="contextMenu" :display="showContextMenu" ref="menu">
      <li @click="appendComponent('parent', $event)">parent</li>
      <li @click="appendComponent('child', $event)">child</li>
      <li @click="appendComponent('note', $event)">note</li>
      <li @click="appendComponent('text', $event)">text</li>
    </context-menu>
  </div>
</template>

<script>
import ContextMenu from "./ContextMenu";
import DraggableInput from "./DraggableInput.vue";
export default {
  name: "StepsMain",
  components: { ContextMenu, DraggableInput },
  data() {
    return { showContextMenu: false, elements: [], id: 0 };
  },
  methods: {
    console() {
      console.log(this.elements);
    },
    openContextMenu(e) {
      this.$refs.menu.open(e);
    },
    appendComponent(type, $event) {
      this.id++;
      console.log($event.clientY, $event.clientX);
      const data = {
        id: this.id,
        type: DraggableInput,
        positionY: $event.pageY || $event.clientY,
        positionX: $event.pageX || $event.clientX,
        relations: [],
      };
      if (type == "parent") {
        let parent = {
          ...data,
          placeholder: "parent",
          background: "#FFCC91",
        };
        this.elements.push(parent);
        this.$refs.menu.close();
      } else if (type == "child") {
        let child = {
          ...data,
          placeholder: "child",
          background: "#91C4FF",
        };
        this.elements.push(child);
        this.$refs.menu.close();
      } else if (type == "note") {
        let note = {
          ...data,
          placeholder: "note",
          background: "#C4C4C4",
        };
        this.elements.push(note);
        this.$refs.menu.close();
      } else if (type == "text") {
        let note = {
          ...data,
          placeholder: "text",
          background: "transparent",
        };
        this.elements.push(note);
        this.$refs.menu.close();
      }
    },
  },
};
</script>

<style>
body {
  margin: 0 !important;
}
.steps-landing {
  min-height: 1300px;
  width: 100%;
  background: black;
  display: flex;
  align-items: center;
  flex-direction: column;
  position: relative;
}
h1 {
  color: white;
}
.logo {
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-drag: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
.container {
  width: 60%;
  height: 1000px;
  background: transparent;
  border: solid 4px white;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
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
  background: #ffcc91;
}
.contextMenu li:nth-child(2) {
  background: #91c4ff;
}
.contextMenu li:nth-child(3) {
  background: #c4c4c4;
}
.contextMenu li:nth-child(4) {
  background: #ffffff;
}
</style>
