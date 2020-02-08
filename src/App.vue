<template>
  <div id="app">
    <Header />
    <b-container class="bv-example-row">
      <b-row class="justify-content-md-center">
        <b-col lg="6">
          <QuestionBox v-if="questions.length" :currentQuestion="questions[index]" :next="next" />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0
    };
  },
  methods: {
    next() {
      this.index++;
    }
  },
  mounted: function() {
    axios
      .get("https://opentdb.com/api.php?amount=10&category=15&type=multiple")
      .then(response => {
        console.log(response);
        this.questions = response.data.results;
      });
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
