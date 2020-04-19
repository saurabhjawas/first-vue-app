<template>
  <div class="question-box-container">
    <b-jumbotron>
    <!-- <template v-slot:header>BootstrapVue</template> -->

    <template v-slot:lead>
      {{ currentQuestion.question }}
    </template>

    <hr class="my-4">

    <b-list-group>
      <b-list-group-item
        v-for="(answer, index) in shuffledAnswers"
        :key="index"
        @click="!answered && selectAnswer(index)"
        :class="answeredClass(index)"
      >
        {{ answer }}
      </b-list-group-item>
    </b-list-group>

    <b-button variant="primary"
      @click="submitAnswer"
      :disabled="selectedIndex === null || answered"
    >
      Submit
    </b-button>
    <b-button variant="success" href="#"
      @click="next"
    >
      Next Question
    </b-button>
  </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null
        this.answered = false
        this.shuffleTheAnswers()        
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    } ,
    shuffleTheAnswers() {
      const answers =  [ ...this.currentQuestion.incorrect_answers , this.currentQuestion.correct_answer ]

      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
      // this.next()
    },
    answeredClass(index) {
      return [
        !this.answered ? 'highlight' : ',',
        !this.answered && this.selectedIndex === index && 'selected' ,
        this.answered && this.correctIndex === index && 'correct' ,
        this.answered && this.selectedIndex === index && index !== this.correctIndex && 'incorrect'
      ]
    }
  }
}
</script>

<style scoped>
  .list-group {
    margin-bottom: 15px;
  }
  .highlight:hover {
    background-color: #eee;
    cursor: pointer;
  }
  .btn {
    margin: 0 5px;
  }
  .selected {
    background-color: lightBlue;
  }
  .correct {
    background-color: lightgreen;
  }
  .incorrect {
    background-color: lightsalmon;
  }
</style>>
