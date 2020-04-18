<template>
  <div class="question-box-container">
    <b-jumbotron>
    <!-- <template v-slot:header>BootstrapVue</template> -->

    <template v-slot:lead>
      {{ currentQuestion.question }}
    </template>

    <hr class="my-4">


    <p v-for="(answer, index) in answers" :key="index">
      {{ answer }}
    </p>

    <b-button variant="primary" href="#">Submit</b-button>
    <b-button variant="success" href="#"
      @click="next"
    >
      Next Question
    </b-button>
  </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function
  },
  computed: {
    answers() {
      return [ ...this.currentQuestion.incorrect_answers , this.currentQuestion.correct_answer]
    }
  }
}
</script>