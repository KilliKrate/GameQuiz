<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{decodeHTML(currentQuestion.question)}}</template>

      <hr class="my-4" />
      <b-list-group class="mb-4">
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="answer"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >{{decodeHTML(answer)}}</b-list-group-item>
      </b-list-group>
      <b-button
        variant="primary"
        @click="submitAnswer()"
        class="mx-2"
        :disabled="selectedIndex === null || answered"
      >Submit</b-button>
      <b-button variant="success" class="mx-2" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import shuffle from "shuffle-array";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false
    };
  },
  methods: {
    selectAnswer(index) {
      if (!this.answered) {
        this.selectedIndex = index;
      }
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = "";
      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        answerClass = "correct";
      } else if (this.answered && this.selectedIndex === index) {
        answerClass = "incorrect";
      }
      return answerClass;
    },
    decodeHTML(html) {
      let txt = document.createElement("textarea");
      txt.innerHTML = html;
      return txt.value;
    }
  },
  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffledAnswers = shuffle(this.answers);
        this.correctIndex = this.shuffledAnswers.indexOf(
          this.currentQuestion.correct_answer
        );
      }
    }
  }
};
</script>

<style>
.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.selected {
  background: lightblue !important;
}

.correct {
  background: lightgreen !important;
}

.incorrect {
  background: red !important;
}
</style>
