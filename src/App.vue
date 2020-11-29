<template>
  <div id="app">
    <Header 
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <Main 
      v-if="questions.length"
      :currentQuestions="questions[index]"
      @next-click="nextQuestions"
      :increment="increment"
    />
  </div>
</template>

<script>
  import Header from '@/layouts/Header'
  import Main from '@/layouts/Main'

  export default {
    name: 'App',
    components: {
      Header,
      Main
    },
    data() {
      return {
        questions: [],
        index: 0,
        numCorrect: 0,
        numTotal: 0
      }
    },
    methods: {
      nextQuestions() {
        if(this.index >= 10) return this.index = 0

        this.index++
      },
      increment(isCorrect) {
        if(isCorrect) this.numCorrect++

        this.numTotal++
      }
    },
    mounted() {
      fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', { method: 'GET' })
        .then(res => res.json())
        .then(data => this.questions = data.results)
        .catch(rej => console.error('EL ERRO ES: ', rej))
    },
  }
</script>

<style lang="scss">
  @import "@/scss/styles.scss";

  *, *::before, *::after{
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  html{
    font-size: 10px;

    body{
      font-family: $font;
      overflow-x: hidden;

      ul li{
        list-style: none;
      }
      
      a, td{
        text-decoration: none;
        font-weight: $font-weight-300;
      }

      h1{
        font-weight: $font-weight-900;
      }
      
      h2{
        font-weight: $font-weight-700;
      }

      h3, h4{
        font-weight: $font-weight-500;
      }

      p{
        font-weight: $font-weight-400;
      }

      span{
        font-weight: $font-weight-100;
      }
    }
  }
</style>

