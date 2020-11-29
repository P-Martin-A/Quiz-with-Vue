<template>
  <main class="main">
    <question-box 
      :currentQuestions="currentQuestions"
      @next-click="$emit('next-click')"
      :increment="increment"
    />
  </main>
</template>

<script>
  import QuestionBox from '@/components/QuestionBox'

  export default {
    name: 'Main',
    components: {
      QuestionBox
    },
    props: {
      currentQuestions: {
        type: Object,
        default: () => {}
      },
      increment: {
        type: Function,
        default: () => { () => {} }
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import "@/scss/styles.scss";
  
  .main{
    background-color: $white;
    @include flex();
    @include size(100%, 90vh);
  }
</style>