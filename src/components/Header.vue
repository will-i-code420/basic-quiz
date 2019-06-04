<template>
  <div class="header-container">
    <b-container class="header-row">
      <b-row class="title" align-h="center">
        <b-col cols="4"><strong>Super Fun Quiz Time</strong></b-col>
      </b-row>
      <b-row class="score" align-h="between">
        <b-col cols="4">Counter: {{ numCorrect }}/{{ numTotal }}</b-col>
        <b-col cols="4">Score: {{ totalScore }}%</b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  props: [
    'numCorrect',
    'numTotal',
    'totalScore'
  ],
  data() {
    return {}
  },
  methods: {
  },
}
</script>

<style scoped>
  .header-container {
    background-color: green
  }
  .title {
    font-size: 28px;
    margin-bottom: 30px;
  }
  .score {
    margin-bottom: 40px;
  }
</style>
