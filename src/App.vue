<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal" :numCurrent="index+1" />
    <b-container class="bv-example-row">
      <b-row class="justify-content-md-center">
        <b-col lg="6">
          <QuestionBox
            v-if="questions.length && index<numTotal-1"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
          <EndScreen
            v-else-if="index >=numTotal-1"
            :numCorrect="numCorrect"
            :numTotal="numTotal"
            :restartGame="restartGame"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import EndScreen from "./components/EndScreen.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
    EndScreen
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 10
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
    },
    restartGame() {
      this.index = 0;
      this.numCorrect = 0;
      this.getQuestions();
    },
    getQuestions() {
      this.questions = [];
      axios
        .get(
          "https://opentdb.com/api.php?amount=" +
            this.numTotal +
            "&category=15&type=multiple"
        )
        .then(response => {
          this.questions = response.data.results;
        });
    }
  },
  mounted: function() {
    this.getQuestions();
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
