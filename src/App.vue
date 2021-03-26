<template>
  <steps-welcome v-if="handler == 'welcome'" @visited="visited"></steps-welcome>
  <steps-app v-if="handler == 'steps'" :stepsData="stepsData"></steps-app>
</template>

<script>
import StepsApp from "./components/StepsApp.vue";
import StepsWelcome from "./components/StepsWelcome.vue";

export default {
  name: "App",
  data() {
    return {
      handler: "welcome",
    };
  },
  methods: {
    visited() {
      this.handler = "steps";
    },
  },
  mounted() {
    if (this.stepsData == null || JSON.parse(this.stepsData)[0].length == 0) {
      this.handler = "welcome";
    } else {
      this.handler = "steps";
    }
  },
  components: {
    StepsApp,
    StepsWelcome,
  },
  computed: {
    stepsData() {
      return localStorage.getItem("stepsData");
    },
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
