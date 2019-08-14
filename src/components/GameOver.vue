<template>
  <b-modal id="game-over" centered title="Game Over">
    <p>Congratulations! You got a {{ totalScore }}.</p>
    <p :class="scoreClass()">Your rank: {{ rank }}</p>
  </b-modal>
</template>

<script>
export default {
  props: [
    'totalScore'
  ],
  data() {
    return {
      rank: ''
    }
  },
  methods: {
    scoreClass() {
      if (this.totalScore > 85) {
        this.rank = "Expert: You know your stuff!!"
      } else if (this.totalScore > 70) {
        this.rank = "Medicore: You have a little learning to do!"
      } else {
        this.rank ="Beginner: Start watching some movies!!!!"
      }
    },
    showModal() {
      this.$refs['game-over'].show()
    },
    hideModal() {
      this.$refs['game-over'].hide()
    },
    mounted() {
      this.showModal()
    }
  }
}
</script>

<style lang="css" scoped>
</style>
