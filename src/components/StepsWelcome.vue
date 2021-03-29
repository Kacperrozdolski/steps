<template>
  <div class="steps-landing">
    <img src="@/assets/logo.svg" alt="" />
    <div class="container">
      <div>
        <h1>STEPS</h1>
        <h2>browser tool for creating simple roadmaps.</h2>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            Right clicking on the workspace frame opens up context menu which
            allows you to create <i>Steps</i> elements.
          </p>
        </div>
        <div class="content-image">
          <img
            src="@/assets/StepsWelcome/context.gif"
            alt=""
            class="smallGif"
          />
        </div>
      </div>
      <div class="content-wrapper" ref="asd">
        <div class="content-text">
          <p>
            First two options are the same generic elements with diffrent
            colors. They are perfect choice for displaying single word element.
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/exampleElement.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            Third option - Note element - creates element which is perfect
            solution for multiline display of text. It's unique feature of
            expanding is avillable in the right bottom corner. You can resize it
            how ever you want.
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/exampleNote.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            Text element allows you to create cell which has a transparent
            background. It's a perfect solution for longer single line
            paragraphs.
            <span style="font-size: 9px; color: #d3d3d3"
              >(this white frame is visable only in instruction)</span
            >
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/exampleText.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            Every cell has it's own menu which allows you to perform some key
            actions like: deleting element from workspace, changing element
            text/body color, creating single line between elements & changing
            current position of an element.
            <span style="font-size: 9px; color: #d3d3d3"
              >(hover on element)</span
            >
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/menu.gif" class="smallGif" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            To change color you have to pick second option is element menu. Then
            type whatever HEX value you want. For example #9c3d54 or #BE0000.
            <span style="font-size: 9px; color: #d3d3d3"
              >(value has to be in HEX notation.)</span
            >
          </p>
        </div>
        <div class="content-image">
          <img
            src="@/assets/StepsWelcome/changecolor.gif"
            class="smallGif"
            alt=""
          />
        </div>
      </div>

      <div class="content-wrapper">
        <div class="content-text">
          <p>
            To connect elements click on the "network" icon on ONE element and
            then click on the another element "network" icon. First selected
            element will float.
            <span style="font-size: 9px; color: #d3d3d3"
              >(second click on the same element recall selection)</span
            >
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/connect.gif" class="bigGif" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            To delete line right click on one. Notice that deleting element with
            attached lines will remove them as well.
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/deletingline.gif" class="bigGif" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            To change element position hold your left click button on the
            "localization" icon and then move your mouse to change element
            position.
            <span style="font-size: 9px; color: #d3d3d3"
              >(be carefull with draging, there is possibility of throwing
              element away)</span
            >
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/drag.gif" class="bigGif" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            After all the tweeking and adjusting press export button which will
            convert - workspace frame - to image file and then automaticly
            download. Carefully everything will be capture <b>exacly</b> how you
            placed it.
            <span style="font-size: 9px; color: #d3d3d3"
              >(exacly means exacly)</span
            >
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/exportButton.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            Oh yes! Don't worry about constant necessity to save. This process
            is automated. So feel free to leave project close browser and return
            to it when ever you want.
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/saveIcon.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper-hero">
        <p class="hero-paragraph">Are you ready? Let's go!</p>
        <button @click="visited">create</button>
      </div>
    </div>
    <div class="footer-container">
      <p>copyright © 2021 kacper rozdolski. all rights reserved.</p>
      <div class="footer-wrapper">
        <img src="@/assets/mail.svg" />
        <img src="@/assets/linkedin.svg" />
        <img src="@/assets/github.svg" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    visited() {
      this.$emit("visited");
    },
  },
};
</script>

<style scoped>
body {
  margin: 0 !important;
}
h1 {
  font-size: 60px;
  margin: 20px 0px 0px 0px;
  text-align: center;
}
h2 {
  font-size: 20px;
  color: rgb(97, 97, 97);
  font-weight: 300;
}
.steps-landing {
  min-height: auto;
  width: 100%;
  background: black;
  display: flex;
  position: relative;
  align-items: center;
  flex-direction: column;
}
.container {
  height: auto;
  max-width: 95vw;
  background: black;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  position: relative;
  border: solid 4px white;
}
.content-wrapper {
  width: 100%;
  height: 100%;
  color: white;
  display: flex;
  align-items: center;
  margin: 10px 0px;
}
.content-text {
  width: 60%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.content-text > p {
  width: 85%;
}
.content-image {
  width: 40%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.hero-paragraph {
  width: 100%;
  text-align: center;
}
.content-wrapper-hero {
  width: 100%;
  height: 100%;
  color: white;
  display: flex;
  align-items: center;
  flex-direction: column;
}
.content-wrapper-hero button {
  width: 100px;
  cursor: pointer;
  margin-bottom: 50px;
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

.bigGif {
  width: 80%;
}
.smallGif {
  width: 60%;
}
</style>