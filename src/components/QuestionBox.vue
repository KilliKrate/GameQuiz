<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{decodeHTML(currentQuestion.question)}}</template>

      <hr class="my-4" />
      <b-list-group class="mb-4">
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="answer"
          @click="submitAnswer(index)"
          :class="answerClass(index)"
        >{{decodeHTML(answer)}}</b-list-group-item>
      </b-list-group>
      <transition name="slide-fade">
        <b-button v-if="answered" variant="success" class="mx-2" @click="next">Next</b-button>
      </transition>
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
    submitAnswer(index) {
      if (!this.answered) {
        this.selectedIndex = index;
        let isCorrect = false;
        if (index === this.correctIndex) {
          isCorrect = true;
        }
        this.answered = true;
        this.increment(isCorrect);
      }
    },
    answerClass(index) {
      let answerClass = "";
      if (this.answered && this.correctIndex === index) {
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

.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>
