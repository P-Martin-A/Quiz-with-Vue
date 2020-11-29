<template>
  <header class="header">
    <nav class="navigation">
      <h1 class="title"
        v-text="title"
      ></h1>
      <h4 class="counter">{{ numCorrect }} / {{ numTotal }}</h4>
    </nav>
  </header>
</template>

<script>
  export default {
    name: 'Header',
    props: {
      numCorrect: {
        type: Number,
        default: () => 0
      },
      numTotal: {
        type: Number,
        default: () => 0
      }
    },
    data() {
      return {
        title: 'Aplicación de Prueba Elegante',
      }
    },
  }
</script>

<style lang="scss" scoped>
  @import "@/scss/styles.scss";
  
  .header{
    color: $white;
    background-color: $green-dark;
    @include size(100%, 10vh);

    .navigation{
      @include flex(space-evenly, center);
      @include size(100%, 100%);
    }
  }
</style>