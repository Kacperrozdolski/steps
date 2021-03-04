<template>
  <steps-main></steps-main>
</template>

<script>
import StepsMain from "./components/StepsMain.vue";

export default {
  name: "App",
  components: {
    StepsMain,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;800&display=swap");
* {
  font-family: "Poppins";
}
::-webkit-scrollbar {
  width: 5px;
  height: 0px;
}
::-webkit-scrollbar-button {
  width: 0px;
  height: 0px;
}
::-webkit-scrollbar-thumb {
  background: #e7d9ea;
  border: 0px none #e7d9ea;
  border-radius: 0px;
}
::-webkit-scrollbar-thumb:hover {
  background: #e7d9ea;
}
::-webkit-scrollbar-thumb:active {
  background: #e7d9ea;
}
::-webkit-scrollbar-track {
  background: #000000;
  border: 0px none #e7d9ea;
  border-radius: 0px;
}
::-webkit-scrollbar-track:hover {
  background: #000000;
}
::-webkit-scrollbar-track:active {
  background: #000000;
}
::-webkit-scrollbar-corner {
  background: transparent;
}
</style>
