<template>
  <div>
    <b-nav>
      <b-nav-item disabled>
        <b>GameQuiz</b>
      </b-nav-item>
      <b-nav-item disabled>Correct Answers: {{this.numCorrect}}/{{this.numTotal}}</b-nav-item>
      <b-nav-item disabled>Question Number {{this.numCurrent}}</b-nav-item>
    </b-nav>
  </div>
</template> 

<script>
export default {
  props: ["numCorrect", "numTotal", "numCurrent"]
};
</script>