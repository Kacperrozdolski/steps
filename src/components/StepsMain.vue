<template>
  <div class="steps-landing">
    <img src="@/assets/logo.svg" @click="console" class="logo" />
    <div
      @contextmenu.prevent="openContextMenu"
      class="container"
      ref="container"
    >
      <component
        :id="this.element.id"
        :is="this.element.type"
        :content="this.element.content"
        :position="this.element.position"
      ></component>
      <!-- <component :is="'NoteInput'"></component>
      <component :is="'ChildInput'"></component>
      <component :is="'TextInput'"></component> -->
    </div>

    <context-menu
      class="contextMenu"
      :display="showContextMenu"
      ref="menu"
    ></context-menu>
  </div>
</template>

<script>
import ChildInput from "./input/ChildInput.vue";
import ContextMenu from "./input/ContextMenu";
import ParentInput from "./input/ParentInput.vue";
import NoteInput from "./input/NoteInput.vue";
import TextInput from "./input/TextInput.vue";

export default {
  name: "StepsMain",
  components: { ContextMenu, ChildInput, ParentInput, NoteInput, TextInput },
  data() {
    return {
      showContextMenu: false,
      element: {
        id: Number,
        type: String,
        connection: [
          {
            connection1: Object,
            connection2: Object,
          },
        ],
        position: {
          postionX: Number,
          postionY: Number,
          innerCenter: {
            // innerX: this.element.id.width / 2,
            innerX: Number,
            // InnerY: this.element.id.height / 2,
            innerY: Number,
          },
        },
        content: String,
      },
    };
  },
  methods: {
    console() {
      console.log(this.element);
    },
    openContextMenu(e) {
      this.$refs.menu.open(e);
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
  width: 1000px;
  height: 1000px;
  background: transparent;
  border: solid 4px white;
  display: flex;
  justify-content: center;
  flex-direction: column;
  overflow: hidden;
  position: relative;
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
