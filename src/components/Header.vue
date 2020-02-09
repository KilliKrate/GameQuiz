<template>
  <div>
    <b-nav>
      <b-nav-item disabled>
        <b>GameQuiz</b>
      </b-nav-item>
      <b-nav-item disabled>Counter: {{this.numCorrect}}/{{this.numTotal}}</b-nav-item>
    </b-nav>
  </div>
</template> 

<script>
export default {
  props: ["numCorrect", "numTotal"]
};
</script>