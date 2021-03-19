<template>
  <div class="steps-landing">
    <img src="@/assets/logo.svg" @click="console" class="logo" />
    <div
      @contextmenu.prevent="openContextMenu"
      class="container"
      ref="container"
    >
      <component
        v-for="element in elements"
        :key="element.id"
        :is="element.type"
        :id="element.id"
        :position="element.position"
        @changePosition="changePosition"
        @establishConnection="establishConnection"
      ></component>
      <svg>
        <component
          v-for="line in lines"
          :is="line.type"
          :key="line.id"
          :position="line.position"
        ></component>
      </svg>
    </div>

    <context-menu
      class="contextMenu"
      :display="showContextMenu"
      ref="menu"
      @orange="orange"
    ></context-menu>
  </div>
</template>

<script>
import ContextMenu from "./input/ContextMenu";
import GenericElement from "./input/GenericElement";
import GenericLine from "./input/GenericLine";

export default {
  name: "StepsMain",
  components: { ContextMenu, GenericElement, GenericLine },
  data() {
    return {
      showContextMenu: false,
      id: -1,
      firstConnection: null,
      secondConnection: null,
      elements: [],
      lines: [],
    };
  },
  methods: {
    openContextMenu(e) {
      this.$refs.menu.open(e);
    },
    changePosition(id, top, left) {
      this.elements[id].position.left = left;
      this.elements[id].position.top = top;
      let i;
      let lines = this.lines.filter(
        (line) => line.betweenElements[0] == id || line.betweenElements[1] == id
      );
      for (i = 0; i < lines.length; i++) {
        if (lines[i].betweenElements[0] == id) {
          lines[i].position.firstEndpoint.left = left;
          lines[i].position.firstEndpoint.top = top;
        }
        if (lines[i].betweenElements[1] == id) {
          lines[i].position.secondEndpoint.left = left;
          lines[i].position.secondEndpoint.top = top;
        }
      }
    },
    orange(e) {
      this.id++;
      let bounds = this.$refs.container.getBoundingClientRect();
      let left = e.clientX - bounds.left;
      let top = e.clientY - bounds.top;
      this.elements.push({
        id: this.id,
        type: "GenericElement",
        position: {
          left: left,
          top: top,
        },
      });
    },
    console() {
      console.log(this.elements);
      console.log(this.lines);
    },
    establishConnection(id) {
      if (this.firstConnection == null) {
        this.firstConnection = id;
      } else if (this.firstConnection != null && id != this.firstConnection) {
        this.secondConnection = id;
        this.lines.push({
          id: 0,
          type: "GenericLine",
          betweenElements: [this.firstConnection, this.secondConnection],
          position: {
            firstEndpoint: {
              left: this.elements[this.firstConnection].position.left,
              top: this.elements[this.firstConnection].position.top,
            },
            secondEndpoint: {
              left: this.elements[this.secondConnection].position.left,
              top: this.elements[this.secondConnection].position.top,
            },
          },
        });
      }
    },
  },
};
</script>

<style>
svg {
  stroke: rgb(255, 255, 255);
  stroke-width: 4;
  width: 100%;
  background: transparent;
  height: 100%;
}
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
  height: 250px;
  background: transparent;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  border: solid 4px white;
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
