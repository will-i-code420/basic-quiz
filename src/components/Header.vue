<template>
  <div class="header-container">
    <b-container class="header-row">
      <b-row class="title" align-h="center">
        <b-col cols="4"><strong>Super Fun Quiz Time</strong></b-col>
      </b-row>
      <b-row class="score" align-h="center">
        <b-col cols="4">
          <b-progress v-if="totalQuestions > 0" :value="numTotal" :max="totalQuestions" class="mb-3"></b-progress>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  props: {
    numTotal: Number,
    totalQuestions: Number
  }
}
</script>

<style scoped>
  .header-container {
    background-color: hsl(195, 100%, 27%);
  }

  .title, .score {
    color: white;
  }

  .title {
    font-size: 28px;
    margin-bottom: 30px;
  }
  .score {
    font-size: 20px;
    margin-bottom: 40px;
  }
</style>
