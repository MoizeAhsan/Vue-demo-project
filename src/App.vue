<template>
  <div id="app">
    <NavBar/>
    <b-container>
      <b-row>
        <b-col sm="12" lg="12">
          <Box
            v-if="index !== null"
            :currentQuestion="questions[index]"
            :currentIndex="index"
            :incrementIdx="incrementIndex"
            :decrementIdx="decrementIndex"
            :totalQuestions="totalQuestions"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Box from "./components/Box.vue";
import NavBar from "./components/NavBar.vue";
import { debuglog } from "util";
export default {
  name: "app",
  components: {
    Box,
    NavBar
  },
  data: function() {
    return {
      questions: [],
      index: null,
      totalQuestions: 0
    };
  },
  methods: {
    incrementIndex() {
      if (this.index >= 0 && this.index < this.questions.length - 1)
        this.index++;
    },
    decrementIndex() {
      if (this.index > 0) this.index--;
    }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&type=multiple", {
      method: "get"
    })
      .then(resp => {
        return resp.json();
      })
      .then(data => {
        this.questions = data.results;
        if (data.results.length) {
          this.index = 0;
          this.totalQuestions = data.results.length;
        }
      });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
