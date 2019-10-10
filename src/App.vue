<template>
  <div id="app">
    <Header
      :numTotal="numTotal"
      :totalQuestions="questions.length"
    />
    <GameStart
    v-if="gameStart"
    :categories="categories"
    :getQuestions="getQuestions"
    />
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="gamePlaying"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :score="score"
            :totalScore="totalScore"
            :totalQuestions="questions.length"
            :numTotal="numTotal"
          />
        </b-col>
      </b-row>
    </b-container>
    <GameOver
    v-if="gameOver"
    :totalScore="totalScore"
    :numTotal="numTotal"
    :numCorrect="numCorrect"
    :newGame="newGame"
    />
  </div>
</template>

<script>
import Header from './components/Header'
import QuestionBox from './components/QuestionBox'
import GameOver from './components/GameOver'
import GameStart from './components/GameStart'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
    GameOver,
    GameStart
  },
  data() {
    return {
      questions: [],
      categories: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      totalScore: 0,
      gameStart: true,
      gamePlaying: false,
      gameOver: false
    }
  },
  methods: {
    next() {
      if (this.numTotal == this.questions.length) {
        this.gamePlaying = false
        this.gameOver = true
      } else {
        return this.index++
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    },
    score() {
      this.totalScore = ( this.numCorrect / this.numTotal ) * 100
    },
    newGame() {
      this.questions = []
      this.index = 0
      this.numCorrect = 0
      this.numTotal = 0
      this.totalScore = 0
      this.gameOver = false
      this.gameStart = true
      this.getCategories()
    },
    getQuestions(category, numOfQuestions, gameDifficulty) {
      fetch(`https://opentdb.com/api.php?amount=${numOfQuestions}&category=${category}&difficulty=${gameDifficulty}&type=multiple`, {
        method: 'get'
      })
      .then((response) => {
        return response.json()
      })
      .then((jsonData) => {
        this.gameStart = false
        this.gamePlaying = true
        this.questions = jsonData.results
      })
    },
    getCategories() {
      fetch('https://opentdb.com/api_category.php', {
        method: 'get'
      }).then(response => {
        return response.json()
      }).then(jsonData => {
        this.categories = jsonData.trivia_categories
      })
    }
  },
  async created() {
    await this.getCategories()
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
