<template>
  <div class="header-container">
    <b-container class="header-row">
      <b-row class="title" align-h="center">
        <b-col v-if="!categoryName" cols="6"><h1>Super Fun Quiz Time</h1></b-col>
        <b-col v-else cols="6"><h1>{{ categoryName }} Quiz</h1></b-col>
      </b-row>
      <b-row class="score" align-h="center">
        <b-col cols="6">
          <b-progress v-if="totalQuestions > 0" :value="numTotal" :max="totalQuestions" variant="warning" class="mb-3"></b-progress>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  props: {
    numTotal: Number,
    totalQuestions: Number,
    categoryName: String
  }
}
</script>

<style scoped>
  .header-container {
    background-color: hsl(195, 100%, 27%);
  }

  .title {
    color: white;
    padding-top: 10px;
    margin-bottom: 20px;
  }

  .score {
    padding-bottom: 10px;
  }
</style>
