<template>
  <div class="questionBox-container">
    <div class="timer" v-if="numTotal < totalQuestions">
      <span>{{ minutes }}:{{ seconds }}</span>
    </div>
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
      <b-button pill v-if="numTotal < totalQuestions" @click="next" variant="success" :disabled="!answered">Next Question</b-button>
      <b-button pill v-else @click="next" variant="success">Get Score</b-button>
      <div class="timer-message">
        {{ timerMessage }}
      </div>
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
      answered: false,
      gameTimer: 10,
      timerMessage: '',
      countdown: null
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      answers = _.shuffle(answers)
      return answers
    },
    minutes() {
      const min = Math.floor(this.gameTimer / 60)
      return this.timerFormat(min)
    },
    seconds() {
      const sec = this.gameTimer - (this.minutes * 60)
      return this.timerFormat(sec)
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
      this.selectedIndex = null
      this.answered = false
      if (this.numTotal == this.totalQuestions) {
          return
        } else {
          this.runGameTimer()
        }
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
      this.resetTimer()
    },
    outOfTime() {
      this.timerMessage = 'Ran Out Of Time!!'
      this.answered = true
      this.increment(false)
      this.score()
      this.resetTimer()
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
    },
    runGameTimer() {
      this.timerMessage = ''
      this.countdown = setInterval(() => {
        this.timeLeft()
      }, 1000)
    },
    timeLeft() {
      if (this.gameTimer >= 1) {
        this.gameTimer--
      } else {
        this.resetTimer()
        this.outOfTime()
      }
    },
    timerFormat(time) {
      return (time < 10 ? '0' : '') + time
    },
    resetTimer() {
      this.gameTimer = 10
      clearInterval(this.countdown)
      this.countdown = null
    }
  }
}
</script>

<style scoped>
  .questionBox-container {
  margin-top: 20px;
  }

  .timer {
    height: 50px;
    width: 100px;
    border: 2px solid white;
    margin: auto;
    color: white;
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
  .timer-message {
    padding-top: 1rem;
    color: red;
    font-size: 22px;
  }
</style>
