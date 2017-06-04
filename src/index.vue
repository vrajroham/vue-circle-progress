<template>
  <div>
    <div :id="id" class="circle">
      <span id="percent-text"></span>
    </div>
  </div>
</template>

<script>
export default {
  props : {
    id: {
        type: String,
        required: true
    },
    progress: {
        type: Number,
        required: true,
        default : 0.25
    },
    size: {
        type: Number,
        required: false,
        default : 150
    },
    startAngle: {
        type: Number,
    },
    thickness: {
        type: Number,
    },
    animationStartValue: {
        type: Number,
        required: false,
        default : 0.0
    },
    reverse:{
      type: Boolean,
      default: false
    },
    lineCap: {
        type: String,
        default : "butt"
    },
    fill:{
      type : Object
    },
    emptyFill:{
      type : String,
    },
    insertMode:{
      type : String,
    },
    showPercent:{
      type : Boolean,
      default : true
    },
    innerText:{
      type : String
    },
  },
  mounted(){
    require('jquery-circle-progress');
    var $ = require('jquery');
    let that = this;
    var el = $('#' + that.id).circleProgress({
      value : this.convertedProgress(that.progress),
      size : that.size,
      startAngle:that.startAngle,
      reverse : that.reverse,
      lineCap: that.lineCap,
      fill: that.fill,
      emptyFill : that.emptyFill,
      animationStartValue:that.animationStartValue,
      insertMode:that.insertMode,
      thickness : that.thickness
    }).on('circle-animation-progress', function(event,progress,stepValue) {
      if ((!that.innerText) && that.showPercent)
        $(this).find('span').html(Math.floor(stepValue*100)+"%");
      else {
        $(this).find('span').html(that.innerText);
      }
      that.$emit('vue-circle-progress', event,progress,stepValue*100);
    })
    .on('circle-animation-end', function(event) {
      that.$emit('vue-circle-end', event);
    });
  },
  methods:{
    convertedProgress : function(progress) {
      return progress/100;
    }
  }
}
</script>

<style scoped>
.circle {
  position: relative;
  display: inline-block;
}

span{
  position: absolute;
 left: 50%;
 top: 50%;
 width: 100px;
 height: 40px;
 margin-left: -50px;
 margin-top: -15px;
 text-align: center;
 font-size-adjust: auto;
}
</style>
