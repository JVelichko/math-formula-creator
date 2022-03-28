<template>
  <div id="app">
    <formula-creator></formula-creator>
  </div>
</template>

<script>

import FormulaCreator from "./components/formula-creator";

export default {
  name: 'App',
  components: {
    FormulaCreator
  }
}
</script>


<style lang="sass">

  @import "src/assets/normalize.sass"
  html, body
    overflow-x: hidden
    font-size: 18px

  $color: purple
  #app
    font-family: Avenir, Helvetica, Arial, sans-serif
    -webkit-font-smoothing: antialiased
    -moz-osx-font-smoothing: grayscale
    text-align: center
    color: #404040

</style>