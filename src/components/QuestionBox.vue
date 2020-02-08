<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>

      <hr class="my-4" />
      <b-list-group class="mb-4">
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="answer"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
        >{{answer}}</b-list-group-item>
      </b-list-group>
      <b-button variant="primary" href="#" class="mx-2">Submit</b-button>
      <b-button variant="success" href="#" class="mx-2" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import shuffle from "shuffle-array";

export default {
  props: {
    currentQuestion: Object,
    next: Function
  },
  data() {
    return {
      selectedIndex: null
    };
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return shuffle(answers);
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
  background: lightgreen;
}

.incorrect {
  background: red;
}
</style>
