<template :data="data">
  <div class="steps-landing">
    <img src="@/assets/logo.svg" class="logo" />
    <div v-if="width < 700" class="error-container fadein">
      <div class="error-popup">
        <img src="@/assets/error.png" alt="" />
        <p>This tool isn't adepted to be used on smaller devices.</p>
      </div>
    </div>
    <div ref="capture">
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
          :content="element.content"
          :color="element.color"
          :position="element.position"
          :placeholder="element.placeholder"
          @changePosition="changePosition"
          @createConnection="createConnection"
          @deleteElement="deleteElement"
          @changeBodycolor="changeBodycolor"
          @changeTextcolor="changeTextcolor"
          @changeContent="changeContent"
        ></component>
        <svg>
          <component
            v-for="line in lines"
            :is="line.type"
            :key="line.id"
            :id="line.id"
            :position="line.position"
            @removeLine="removeLine"
          ></component>
        </svg>
        <p class="remove-button" @click="remove">delete project</p>
      </div>
    </div>
    <div class="button-container">
      <button @click="createImage">export</button>
    </div>
    <div class="footer-container">
      <p>copyright © 2021 kacper rozdolski. all rights reserved.</p>
      <div class="footer-wrapper">
        <img src="@/assets/mail.svg" />
        <img src="@/assets/linkedin.svg" />
        <img src="@/assets/github.svg" />
      </div>
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
import GenericNote from "./input/GenericNote";
import GenericText from "./input/GenericText";

export default {
  name: "StepsMain",
  components: {
    ContextMenu,
    GenericElement,
    GenericLine,
    GenericNote,
    GenericText,
  },
  props: ["stepsData"],
  data() {
    return {
      showContextMenu: false,
      elements: [],
      id: 0,
      lines: [],
      width: 0,
      lineId: 0,
      firstConnection: null,
      secondConnection: null,
      firstElementIndex: null,
      secondElementIndex: null,
      css:
        "svg{stroke:#fff;stroke-width:4;width:100%;background:transparent;height:100%}body{margin:0!important}.steps-landing{min-height:150vh;width:100%;background:black;display:flex;position:relative;align-items:center;flex-direction:column}h1{color:white}.logo{-moz-user-select:none;-webkit-user-select:none;-ms-user-select:none;-o-user-select:none;user-select:none;user-select:none;-moz-user-select:none;-webkit-user-drag:none;-webkit-user-select:none;-ms-user-select:none}.container{width:900px;height:900px;background:black;display:flex;justify-content:center;align-items:center;position:relative;border:solid 4px white}.draggable-container{position:absolute}div:hover>menu{top:-35px}.genericNote{min-width:180px;min-height:90px;border:0;padding:0;text-decoration:none!important;position:relative;z-index:1;text-align:center;cursor:pointer}.genericNote:focus{outline:0}.genericNote::placeholder{color:var(--color)}menu{width:100%;height:35px;background:#fff;position:absolute;margin:0;transition:top .5s;top:0;padding:0;display:flex;justify-content:space-around;align-items:center}img{height:23px;cursor:pointer;user-select:none;-moz-user-select:none;-webkit-user-drag:none;-webkit-user-select:none;-ms-user-select:none}.palete{position:absolute;width:100%;height:60px;bottom:100%;margin:0;background:#fff}.colorInput{width:100%;height:50%;margin:0;display:flex;align-items:center;justify-content:space-evenly}.colorInput input{width:60%;height:23px;margin:0;background:#dfdfdf;border:0}.textColor{height:25px;width:25px}.draggable-container{position:absolute}div:hover>menu{top:-35px}.genericElement{width:140px;height:60px;text-decoration:none!important;border:0;padding:0;position:relative;z-index:1;text-align:center;cursor:pointer}input:focus{outline:0}.genericElement::placeholder{color:var(--color)}menu{width:100%;height:35px;background:#fff;position:absolute;margin:0;transition:top .5s;top:0;padding:0;display:flex;justify-content:space-around;align-items:center}img{height:23px;cursor:pointer;user-select:none;-moz-user-select:none;-webkit-user-drag:none;-webkit-user-select:none;-ms-user-select:none}.palete{position:absolute;width:100%;height:60px;bottom:100%;margin:0;background:#fff}.colorInput{width:100%;height:50%;margin:0;display:flex;align-items:center;justify-content:space-evenly}.colorInput input{width:60%;height:23px;margin:0;background:#dfdfdf;border:0}.textColor{height:25px;width:25px}line{cursor:pointer;position:absolute;background:transparent} .draggable-container{position:absolute}#menuText{top:-30px}.genericText{width:140px;height:60px;text-decoration:none!important;border:0;padding:0;position:relative;z-index:1;text-align:center;cursor:pointer}input:focus{outline:0}.genericText::placeholder{color:var(--color)}menu{width:100%;height:35px;background:#fff;position:absolute;margin:0;transition:top .5s;top:0;padding:0;display:flex;justify-content:space-around;align-items:center}img{height:23px;cursor:pointer;user-select:none;-moz-user-select:none;-webkit-user-drag:none;-webkit-user-select:none;-ms-user-select:none}.palete{position:absolute;width:100%;height:60px;bottom:100%;margin:0;background:#fff}.colorInput{width:100%;height:50%;margin:0;display:flex;align-items:center;justify-content:space-evenly}.colorInput input{width:60%;height:23px;margin:0;background:#dfdfdf;border:0}.textColor{height:25px;width:25px}",
    };
  },
  methods: {
    openContextMenu(e) {
      this.$refs.menu.open(e);
    },
    appendElement(e, element) {
      let bounds = this.$refs.container.getBoundingClientRect();
      let left = e.clientX - bounds.left;
      let top = e.clientY - bounds.top;
      if (element == "yellow") {
        this.elements.push({
          id: this.id++,
          type: "GenericElement",
          position: {
            left: left,
            top: top,
          },
          color: { bodyColor: "#e29e00", textColor: "#000000" },
          content: "",
          placeholder: "Yellow",
        });
      } else if (element == "green") {
        this.elements.push({
          id: this.id++,
          type: "GenericElement",
          position: {
            left: left,
            top: top,
          },
          color: { bodyColor: "#00AF91", textColor: "#000000" },
          content: "",
          placeholder: "Green",
        });
      } else if (element == "note") {
        this.elements.push({
          id: this.id++,
          type: "GenericNote",
          position: {
            left: left,
            top: top,
          },
          color: { bodyColor: "#c0c0c0", textColor: "#000000" },
          content: "",
          placeholder: "Note",
        });
      } else if (element == "text") {
        this.elements.push({
          id: this.id++,
          type: "GenericText",
          position: {
            left: left,
            top: top,
          },
          color: { bodyColor: "transparent", textColor: "#FFF" },
          content: "",
          placeholder: "Text",
        });
      }
      this.save();
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
      this.save();
    },
    changeContent(id, content) {
      let element = this.elements.find((element) => element.id == id);
      let index = this.elements.indexOf(element);
      this.elements[index].content = content;
      this.save();
    },
    createConnection(id) {
      if (this.firstConnection == null) {
        let element = this.elements.find((element) => element.id == id);
        let index = this.elements.indexOf(element);
        this.firstElementIndex = index;
        this.firstConnection = id;
        document.getElementById(id).classList.add("floating");
      } else if (this.firstConnection == id) {
        document
          .getElementById(this.firstConnection)
          .classList.remove("floating");
        this.firstConnection = null;
      } else if (
        (this.firstConnection != null) &
        (this.firstConnection != id)
      ) {
        document
          .getElementById(this.firstConnection)
          .classList.remove("floating");
        let element = this.elements.find((element) => element.id == id);
        let index = this.elements.indexOf(element);
        this.secondElementIndex = index;
        this.secondConnection = id;
        this.lines.push({
          id: this.lineId++,
          type: "GenericLine",
          betweenElements: [this.firstConnection, this.secondConnection],
          position: {
            firstEndpoint: {
              left: this.elements[this.firstElementIndex].position.left,
              top: this.elements[this.firstElementIndex].position.top,
            },
            secondEndpoint: {
              left: this.elements[this.secondElementIndex].position.left,
              top: this.elements[this.secondElementIndex].position.top,
            },
          },
        });
        this.save();
        this.firstConnection = null;
        this.secondConnection = null;
      }
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
      if (id == this.firstConnection) {
        this.firstConnection = null;
      }
      this.save();
    },
    save() {
      let stepsData = [this.elements, this.lines];
      localStorage.setItem("stepsData", JSON.stringify(stepsData));
    },
    remove() {
      localStorage.removeItem("stepsData");
      this.elements = [];
      this.id = 0;
      this.lines = [];
      this.lineId = 0;
    },
    async createImage() {
      const payload = {
        html: this.$refs.capture.innerHTML,
        css: this.css,
      };
      let headers = {
        auth: {
          username: "1a9f8520-662e-49af-b33d-d8fa90836e2c",
          password: "10325606-73fa-4806-975e-11337bd4d039",
        },
        headers: {
          "Content-Type": "application/json",
        },
      };
      try {
        const response = await this.axios.post(
          "https://hcti.io/v1/image",
          JSON.stringify(payload),
          headers
        );
        this.downloadImage(response.data.url);
      } catch (error) {
        console.error(error);
      }
    },
    downloadImage(url) {
      this.axios({
        url: url,
        method: "GET",
        responseType: "blob",
      }).then((response) => {
        var fileURL = window.URL.createObjectURL(new Blob([response.data]));
        var fileLink = document.createElement("a");
        fileLink.href = fileURL;
        fileLink.setAttribute("download", "steps-image.jpg");
        document.body.appendChild(fileLink);
        fileLink.click();
      });
    },
    changeBodycolor(id, color) {
      let element = this.elements.find((element) => element.id == id);
      let index = this.elements.indexOf(element);
      this.elements[index].color.bodyColor = color;
      this.save();
    },
    changeTextcolor(id, color) {
      let element = this.elements.find((element) => element.id == id);
      let index = this.elements.indexOf(element);
      this.elements[index].color.textColor = color;
      this.save();
    },
    removeLine(id) {
      let line = this.lines.find((line) => line.id == id);
      let index = this.lines.indexOf(line);
      this.lines.splice(index, 1);
      this.save();
    },
    handleResize() {
      this.width = window.innerWidth;
    },
  },
  mounted() {
    window.addEventListener("resize", this.handleResize);
    this.handleResize();
    console.log();
    window.scrollTo({ top: 0, behavior: "smooth" });
    let data = this.stepsData;
    data = JSON.parse(data);
    if (data == null || data[0] == 0) {
      console.log("Brak danych");
    } else {
      if (data[0].length != 0) {
        this.elements = data[0];
        this.id = this.elements[this.elements.length - 1].id + 1;
      }
      if (data[1].length != 0) {
        this.lines = data[1];
        this.lineId = this.lines[this.lines.length - 1].id + 1;
      }
    }
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
  min-height: 150vh;
  width: 100%;
  background: black;
  display: flex;
  position: relative;
  align-items: center;
  flex-direction: column;
  overflow: hidden;
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
  height: 900px;
  width: 1000px;
  background: black;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  border: solid 4px white;
}
@media only screen and (max-width: 1100px) {
  .container {
    width: 91vw;
  }
}
.footer-container {
  height: 200px;
  width: 100%;
  color: #e7d9ea;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.footer-container img {
  margin: 10px;
}
.footer-container > p {
  text-align: center;
}
.button-container {
  width: 100%;
  height: 20vh;
  color: white;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
.button-container button {
  width: 130px;
  height: 35px;
  cursor: pointer;
}
.remove-button {
  position: absolute;
  bottom: -35px;
  font-weight: 200;
  right: 0;
  font-size: 10px;
  cursor: pointer;
  color: grey;
  z-index: 0;
}
.error-container {
  width: 100%;
  height: 100%;
  position: absolute;
  z-index: 4;
  display: flex;
  background: rgba(0, 0, 0, 0.7);
  flex-direction: column;
  align-items: center;
}
.error-popup {
  margin-top: 20vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: white;
  width: 100%;
}
.error-popup img {
  width: 75vw;
}
.error-popup p {
  font-size: 12px;
  width: 70%;
  text-align: center;
}
.fadein {
  -webkit-animation: fadein 2s; /* Safari, Chrome and Opera > 12.1 */
  -moz-animation: fadein 2s; /* Firefox < 16 */
  -ms-animation: fadein 2s; /* Internet Explorer */
  -o-animation: fadein 2s; /* Opera < 12.1 */
  animation: fadein 2s;
}

@keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
