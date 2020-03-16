<template>
  <div class="game-start">
    <h1 class="game-start-title">Pick Your Poison</h1>
      <b-card-group deck v-for="row in mapCategories" :key="row.id" class="category-cards">
        <b-card v-for="category in row" :key="category.id" :title="category.name">
          <b-card-text>
            <label>Select Difficulty Level:
              <b-form-radio-group
                :name="category.name"
                @change="setGame(category.id, $event)"
              >
              <b-form-radio value='easy'>
                Easy
              </b-form-radio>
              <b-form-radio value='medium'>
                Medium
              </b-form-radio>
              <b-form-radio value='hard'>
                Hard
              </b-form-radio>
              </b-form-radio-group>
            </label>
            <label v-if="category.id === selectedCategory">Select Question Amount:
              <b-form-input
                v-model="selectedQuestionAmt"
                :name="category.name"
                type="range"
                min="5"
                :max="questionMax"
                number
              >
              </b-form-input>
            </label>
          </b-card-text>
          <template v-slot:footer>
            <b-button pill variant="primary" @click="startGame(category.name)">
              Start Game
            </b-button>
          </template>
        </b-card>
      </b-card-group>
  </div>
</template>

<script>
export default {
  props: {
    categories: Array
  },
  data() {
    return {
      selectedCategory: '',
      selectedDifficulty: '',
      selectedQuestionAmt: 5,
      questionMax: '',
      options: [
        { text: 'Easy', value: 'easy'},
        { text: 'Medium', value: 'medium'},
        { text: 'Hard', value: 'hard'},
      ]
    }
  },
  computed: {
    mapCategories() {
      let sortedCategories = [...this.categories]
      return sortedCategories.reduce((a, b, i) => {
        if (i % 3 === 0) a.push([])
        a[a.length - 1].push(b)
        return a
      }, [])
    }
  },
  methods: {
    async startGame(name) {
      try {
        await this.$emit('start-game', this.selectedCategory, this.selectedQuestionAmt, this.selectedDifficulty, name)
      } catch (err) {
        alert(`Error: ${err}`)
      }
    },
    setGame(id, difficulty) {
      this.selectedCategory = id
      this.selectedDifficulty = difficulty
      this.selectedQuestionAmt = 5
      this.getNumberOfQuestions()
    },
    async getNumberOfQuestions() {
      const questionQuery = await fetch(`https://opentdb.com/api_count.php?category=${this.selectedCategory}`, {
        method: 'get'
      }).then(res => {
        return res.json()
      }).then(jsonData => {
        return jsonData.category_question_count
      })
      switch(this.selectedDifficulty) {
        case 'easy':
        this.questionMax = questionQuery.total_easy_question_count
        break;
        case 'medium':
        this.questionMax = questionQuery.total_medium_question_count
        break;
        case 'hard':
        this.questionMax = questionQuery.total_hard_question_count
        break;
      }
    }
  }
}
</script>

<style scoped>
.category-cards {
  margin: 30px 0;
}
</style>
