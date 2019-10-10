<template>
  <div class="game-over">
    <h1>Game Over Screen</h1>
    <p>You got {{ numCorrect }} out of {{ numTotal }}</p>
    <p>Your score is {{ totalScore }}%</p>
    <b-button variant="primary" @click="newGame">Play Again</b-button>
  </div>
</template>

<script>
export default {
  props: {
    totalScore: Number,
    numCorrect: Number,
    numTotal: Number,
    newGame: Function
  }
}
</script>

<style lang="css" scoped>
</style>
