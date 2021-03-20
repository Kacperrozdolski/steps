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
        @deleteElement="deleteElement"
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
      @appendElement="appendElement"
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
      id: 0,
      lineId: 0,
      firstConnection: null,
      secondConnection: null,
      elements: [],
      lines: [],
    };
  },
  methods: {
    openContextMenu(e) {
      this.$refs.menu.open(e);
      console.log("Otworzyłem Menu");
    },
    appendElement(e) {
      let bounds = this.$refs.container.getBoundingClientRect();
      let left = e.clientX - bounds.left;
      let top = e.clientY - bounds.top;
      this.elements.push({
        id: this.id++,
        type: "GenericElement",
        position: {
          left: left,
          top: top,
        },
        color: { bodyColor: "#f3334", textColor: "#f3f33" },
        content: "",
      });
      console.log("Stworzyłem element");
    },
    changePosition(id, top, left) {
      let element = this.elements.find((element) => element.id == id);
      let index = this.elements.indexOf(element);
      this.elements[index].position.left = left;
      this.elements[index].position.top = top;
      let i;
      let lineArray = this.lines.filter(
        (line) => line.betweenElements[0] == id || line.betweenElements[1] == id
      );
      for (i = 0; i < lineArray.length; i++) {
        let lineIndex = this.lines.indexOf(lineArray[i]);
        if (this.lines[lineIndex].betweenElements[0] == id) {
          this.lines[lineIndex].position.firstEndpoint.left = left;
          this.lines[lineIndex].position.firstEndpoint.top = top;
        }
        if (this.lines[lineIndex].betweenElements[1] == id) {
          this.lines[lineIndex].position.secondEndpoint.left = left;
          this.lines[lineIndex].position.secondEndpoint.top = top;
        }
      }
      console.log("Zmieeeniaaam pozycję");
    },
    deleteElement(id) {
      let element = this.elements.find((element) => element.id == id);
      let index = this.elements.indexOf(element);
      this.elements.splice(index, 1);
      let i;
      let lineArray = this.lines.filter(
        (line) => line.betweenElements[0] == id || line.betweenElements[1] == id
      );
      for (i = 0; i < lineArray.length; i++) {
        let lineIndex = this.lines.indexOf(lineArray[i]);
        this.lines.splice(lineIndex, 1);
      }
      console.log("Usunąłem Element");
    },
    establishConnection(id) {
      if (this.firstConnection == null) {
        let element = this.elements.find((element) => element.id == id);
        let index = this.elements.indexOf(element);
        this.firstConnection = index;
        console.log("Znalazłem pierwszy element do złączenia");
      }
      if (this.firstConnection != null && id != this.firstConnection) {
        let element = this.elements.find((element) => element.id == id);
        let index = this.elements.indexOf(element);
        this.secondConnection = index;
        this.lines.push({
          id: this.lineId++,
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
        this.firstConnection = null;
        this.secondConnection = null;
        console.log("Nawiązałem Połączenie");
      }
    },
    console() {
      let lineArray = this.lines.filter(
        (line) => line.betweenElements[0] == 0 || line.betweenElements[1] == 0
      );
      let lineArray1 = this.lines.filter(
        (line) => line.betweenElements[0] == 1 || line.betweenElements[1] == 1
      );
      let lineArray2 = this.lines.filter(
        (line) => line.betweenElements[0] == 2 || line.betweenElements[1] == 2
      );
      console.log(lineArray, lineArray1, lineArray2);
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
  width: 900px;
  height: 700px;
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
