<template>
  <div class="game-start">
    <h1 class="game-start-title">Pick Your Poison</h1>
      <b-card-group deck v-for="row in mapCategories" :key="row.id" class="category-cards">
        <b-card v-for="category in row" :key="category.id" :title="category.name">
          <b-card-text>
            <label>Select Amount Of Questions</label>
            <b-form-input v-model="questionAmount[category.id]" type="range" min="5" max="50"></b-form-input>
            <span>{{ questionAmount[category.id] }} Question Quiz</span>
            <hr>
            <label>Select Difficulty Level:</label>
            <b-form-radio-group
            v-model="difficulty[category.id]"
            :options="options"
            ></b-form-radio-group>
          </b-card-text>
          <template v-slot:footer>
            <b-button pill variant="primary" @click="startGame(category.id, category.name)">Start Game</b-button>
          </template>
        </b-card>
      </b-card-group>
  </div>
</template>

<script>
export default {
  props: {
    categories: Array,
    getQuestions: Function
  },
  data() {
    return {
      questionAmount: {},
      difficulty: {},
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
    async startGame(categoryId, categoryName) {
      try {
        let numOfQuestions = await Number(this.filterSelection(this.questionAmount, categoryId))
        let gameDifficulty = await this.filterSelection(this.difficulty, categoryId).join()
        await this.getQuestions(categoryId, numOfQuestions, gameDifficulty, categoryName)
      } catch (err) {
        alert(`Error: ${err}`)
      }
    },
    filterSelection(obj, value) {
      let itemsToFilter = obj
      itemsToFilter = Object.keys(obj).filter(key => {
        return obj[key] == obj[value]
      }).map(key => {
        return obj[key]
      })
      return itemsToFilter
    }
  }
}
</script>

<style scoped>
.category-cards {
  margin: 30px 0;
}
</style>
