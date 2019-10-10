<template>
  <div class="game-over">
    <h1>Quiz Over</h1>
    <h2 class="game-over-questions">You got {{ numCorrect }} out of {{ numTotal }}</h2>
    <h2 class="game-over-score">Your score is {{ totalScore }}%</h2>
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

<style scoped>
.game-over {
  margin-top: 10px;
  height: 80vh;
  color: white;
}

h1 {
  font-size: 60px;
}

.game-over-questions {
  margin-top: 40px;
}

.game-over-score {
  margin-top: 60px;
}

.btn {
  margin-top: 50px;
}
</style>
