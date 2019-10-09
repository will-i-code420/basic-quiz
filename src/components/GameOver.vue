<template>
  <div class="game-over">
    <h1>Game Over Screen</h1>
    <p>{{ totalScore }}</p>
    <b-button variant="primary" @click="newGame">Play Again</b-button>
  </div>
</template>

<script>
export default {
  props: {
    totalScore: Number,
    newGame: Function
  },
  methods: {
  }
}
</script>

<style lang="css" scoped>
</style>
