<template>
  <div class="game-over">
    <h1>Quiz Over</h1>
    <h2>You got {{ numCorrect }} out of {{ numTotal }}</h2>
    <h2>Your score is {{ totalScore }}%</h2>
    <b-button pill variant="primary" @click="newGame">New Game</b-button>
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
.game-over {
  margin-top: 10px;
  height: 80vh;
}
</style>
