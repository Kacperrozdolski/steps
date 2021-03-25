<template>
  <div class="steps-landing">
    <img src="@/assets/logo.svg" alt="" />
    <div class="container">
      <h1>STEPS</h1>
      <h2>webtool for creating simple roadmap.</h2>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            Right clicking in the workspace frame opens up context menu which allows
            you to create roadmap elements.
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/contextmenu.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            First two options creates basic cells which are your main tools to
            create amazing roadmap. Perfect choice for single word display of
            text.
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/exampleElement.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            Note option create bigger cell which can be used as a container for
            bigger chunks of text. Their uniqe behavior is that if u grab it by
            the right bottom corner u can resize it how ever you want.
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/exampleNote.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            Text element allows you to create cell which has default transparent
            background. It’s a perfect solution for straight line paragraphs.
            (frame is only in instruction)
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/exampleText.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            Every cell comes with menu which allows you to: perform certein
            actions like: removing, changing color, creating realtion with other
            element or change current postiton of an cell.
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/exampleMenu.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            After all the tweeking and adjusting press export button which will
            convert workspace frame to image file. Be carefully everything will
            be capture exacly how you placed it.
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/exportButton.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <div class="content-text">
          <p>
            Oh yes! I almost forgot dont worry about constantly saving
            everything every 3 minute. This procces is automated so feel free to
            leave project and return when ever you want.
          </p>
        </div>
        <div class="content-image">
          <img src="@/assets/StepsWelcome/saveIcon.svg" alt="" />
        </div>
      </div>
      <div class="content-wrapper">
        <p class="hero-paragraph">Are you ready? Let's go!</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
body {
  margin: 0 !important;
}
h1 {
  font-size: 60px;
  margin: 40px 0px 0px 0px;
}
h2 {
  font-size: 20px;
  color: rgb(97, 97, 97);
  margin: 0 0 50px 0;
  font-weight: 300;
}
.steps-landing {
  min-height: 150vh;
  width: 100%;
  background: black;
  display: flex;
  position: relative;
  align-items: center;
  flex-direction: column;
}
.container {
  min-height: 1300px;
  width: 900px;
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
}
.content-text {
  width: 65%;
  min-height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.content-text > p {
  width: 85%;
}
.content-image {
  width: 50%;
  min-height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.hero-paragraph {
  width: 100%;
  text-align: center;
}
</style>