<template>
  <div class="block">
    <div class="question">
      <QuizSite msg="Welcome to ITIL quiz"/>
    </div>
  </div>
</template>

<script>
import QuizSite from './components/QuizSite.vue'

export default {
  name: 'App',
  components: {
    QuizSite
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  font-size: 20px;
  color: #2c3e50;
  margin-top: 60px;

}

.block {
  display: flex;
  align-items: center;
  width: 100%;
  margin-top: 90px;
  padding: 20px;
}

.question {
  width: 60%;
  margin: 0 auto;
  text-align: center;
}
</style>
