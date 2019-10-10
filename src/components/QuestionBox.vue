<template>
  <div class="questionBox-container">
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item v-for="(answer, index) in answers" :key="index" @click="selectAnswer(index)" :class="answerClass(index)">
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button pill v-if="numTotal < totalQuestions" variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered">Submit</b-button>
      <b-button pill v-if="numTotal < totalQuestions" @click="next" variant="success">Next Question</b-button>
      <b-button pill v-else @click="next" variant="success">Get Score</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
    score: Function,
    totalQuestions: Number,
    numTotal: Number
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      answers = _.shuffle(answers)
      return answers
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
      this.selectedIndex = null
      this.answered = false
      }
    },
    answers: {
      immediate: true,
      handler(correctAnswer) {
        this.shuffledAnswers = correctAnswer
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    },
    submitAnswer() {
      let isCorrect = false

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
      this.score()
    },
    answerClass(index) {
      let answerClass = ''

      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected"
      } else if (this.answered && this.correctIndex === index) {
        answerClass = "correct"
      } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
        answerClass = "incorrect"
      }
      return answerClass
    }
  }
}
</script>

<style scoped>
.questionBox-container {
  margin-top: 20px;
}
  .list-group {
    margin-bottom: 15px;
  }
  .list-group-item:hover {
    background: #eee;
    cursor: pointer;
  }
  .btn {
    margin: 0 5px;
  }
  .selected {
    background-color: blue;
  }
  .correct {
    background-color: green;
  }
  .incorrect{
    background-color: red;
  }
</style>
