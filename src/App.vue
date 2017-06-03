<template>
  <div id="app">
    <p>
      A Vue.js component to draw animated circular progress bars!
    </p>
      <vue-circle id="circle"></vue-circle>
  </div>
</template>

<script>
  import VueCircle from '../src/index.vue'
  export default {
    components: {
      VueCircle
    }
  }
</script>

<style scoped>
  /*@import url('~dropzone/dist/dropzone.css');*/
  @import 'https://fonts.googleapis.com/css?family=Roboto';
</style>
