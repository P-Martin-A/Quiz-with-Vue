<template>
  <section class="container">
    <div class="box">
      <h2 class="title"
        v-text="currentQuestions.question"
      ></h2>
      <ul class="answers">
        <li class="answer"
          v-for="(answer, index) in answers" 
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
          v-text="answer"
        ></li>
      </ul>
    </div>
    <div class="buttons">
      <button class="submit"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
        v-text="submit"
      ></button>
      <button class="next"
        @click="$emit('next-click')"
        v-text="next"
      ></button>
    </div>
  </section>
</template>

<script>
  import _ from 'lodash'

  export default {
    name: 'QuestionBox',
    props: {
      currentQuestions: {
        type: Object,
        default: () => {}
      },
      increment: {
        type: Function,
        default: () => { () => {} }
      }
    },
    data() {
      return {
        submit: 'Enviar',
        next: 'Siguiente',
        selectedIndex: null,
        correctIndex: null,
        shuffledAnswers: [],
        answered: false,
      }
    },
    computed: {
      answers() {
        const ANSWERS = [...this.currentQuestions.incorrect_answers]
        
        ANSWERS.push(this.currentQuestions.correct_answer)
        
        return ANSWERS
      }
    },
    watch: {
      currentQuestions: {
        immediate: true,
        handler() {
          this.selectedIndex = null
          this.answered = false
          this.shuffleAnswers()
        }
      }
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index
      },
      shuffleAnswers(){
        const ANSWERS = [...this.currentQuestions.incorrect_answers, this.currentQuestions.correct_answer]
       
        this.shuffledAnswers = _.shuffle(ANSWERS)

        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestions.correct_answer)
      },
      submitAnswer() {
        let isCorrect = false

        if(this.selectedIndex === this.correctIndex) isCorrect = true

        this.answered = true

        this.increment(isCorrect)
      },
      answerClass(index) {
        let answerClass = ''

        if(!this.answered && this.selectedIndex === index) return answerClass = 'selected'
        
        if(this.answered && this.correctIndex === index) return answerClass = 'correct'
        
        if(this.answered && this.selectedIndex === index && this.correctIndex !== index) return answerClass = 'incorrect'
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import "@/scss/styles.scss";
  
  .container{
    padding: 5rem 2rem;
    background-color: $black;
    @include size(100%, 100%);

    @include desktops() {
      padding: 4rem;
      @include size(50rem, 50rem);
    }

    .box{
      color: $white;
      @include size(100%, 90%);

      .title{
        font-size: 6vw;
        @include size(100%, 40%);

        @include desktops() {
          font-size: 2vw;
        }
      }

      .answers{
        text-align: center;
        @include size(100%, 60%);

        .answer{
          color: $grey-light;
          font-size: 6vw;
          margin: .5rem 0;
          padding: .6rem;
          background-color: $white;
          cursor: pointer;
          @include desktops() {
            font-size: 2.5rem;
            padding: 0;
          }

          &:hover{
            background-color: $grey-dark;
          }

          &.selected, &.correct, &.incorrect{
            color: $white;
          }

          &.selected{
            background-color: $blue-light;
          }

          &.correct{
            background-color: $green-light;
          }

          &.incorrect{
            background-color: $red-light;
          }
        }
      }
    }

    .buttons{
      @include size(100%, 10%);
      @include flex(space-evenly, center);

      .submit, .next{
        padding: 1rem 2rem;
        color: $white;
        border: none;
        border-radius: .5rem;
        cursor: pointer;
      }

      .submit{
        background-color: $blue-light;

        &:hover{
          background-color: $blue-dark;
        }
      }

      .next{ 
        background-color: $green-light;

        &:hover{
          background-color: $green-dark;
        }
      }
    }
  }
</style>
