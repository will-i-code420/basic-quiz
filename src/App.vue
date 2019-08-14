<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :score="score"
            :gameStatus="gameStatus"
            :totalScore="totalScore"
          />
          <GameOver
          v-else-if="gameStatus"
          :totalScore="totalScore"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import GameOver from './components/GameOver.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
    GameOver
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      totalScore: 0,
      gameStatus: false
    }
  },
  methods: {
    next() {
      if (this.numTotal == this.questions.length) {
        return
      } else {
        return this.index++
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
      this.gameOver()
    },
    score() {
      this.totalScore = ( this.numCorrect / this.numTotal ) * 100
    },
    gameOver() {
      if (this.numTotal == this.questions.length) {
        this.gameStatus = true
      }
    },
    newGame() {
      this.questions = []
      this.index = 0
      this.numCorrect = 0
      this.numTotal = 0
      this.totalScore = 0
      this.gameStatus = false
      this.getQuestions()
    },
    getQuestions() {
      fetch('https://opentdb.com/api.php?amount=10&category=11&difficulty=easy&type=multiple', {
        method: 'get'
      })
      .then((response) => {
        return response.json()
      })
      .then((jsonData) => {
        this.questions = jsonData.results
      })
    }
  },
  created() {
    this.getQuestions()
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-image: linear-gradient(hsl(210, 28%, 52%), hsl(210, 13%, 50%));
}
</style>
