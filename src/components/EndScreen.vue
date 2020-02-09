<template>
  <div>
    <b-jumbotron>
      <template v-slot:header>Good Game!</template>

      <template v-slot:lead>You got {{numCorrect}} out of {{numTotal}} correctly!</template>

      <hr class="my-4" />

      <b-button variant="primary" @click="restartGame()">Again?</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    numCorrect: Number,
    numTotal: Number,
    restartGame: Function
  }
};
</script>