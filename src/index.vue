<template>
  <div>
    <div :id="id"></div>
  </div>
</template>

<script>
export default {
  props : ['id'],
  mounted(){
    require('jquery-circle-progress');
    var $ = require('jquery');
    $('#circle').circleProgress({
      value: 0.75
    });
  }
}
</script>

<style>
  h1 {
    color: #333;
  }
</style>
