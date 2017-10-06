<template>
  <div>
    <div :id="id" class="circle">
      <div class="circle-percent-text-body">
        <span class="percent-text" :style="{ 'font-size': percentFontSize }"></span>
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
var $ = require('jquery');
require('jquery-easing')($);
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
    percentFontSize: {
      type: String,
      required: false,
      default: function(){
        return (this.size / 4).toString() + "px";
      } 
    },
    animation: {
        required: false,
        default: function(){
          return { duration: 1200, easing: "circleProgressEasing" };
        },
        validator: function(value){
          return (value instanceof Object || value === false);
        }
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
  },
  mounted(){
    require('jquery-circle-progress');
    let that = this;
    let el = $('#' + that.id)
    .on('circle-inited', function(event){
      renderCircleBody(this, (that.progress/100));
      that.$emit('vue-circle-init', event);
    })
    .circleProgress({
      value : this.convertedProgress(that.progress),
      size : that.size,
      startAngle:that.startAngle,
      reverse : that.reverse,
      lineCap: that.lineCap,
      fill: that.fill,
      emptyFill : that.emptyFill,
      animation: that.animation,
      animationStartValue:that.animationStartValue,
      insertMode:that.insertMode,
      thickness : that.thickness
    })
    .on('circle-animation-progress', function(event,progress,stepValue) {
      renderCircleBody(this, stepValue);
      that.$emit('vue-circle-progress', event,progress,stepValue*100);
    })
    .on('circle-animation-end', function(event) {
      that.$emit('vue-circle-end', event);
    });

    function renderCircleBody(self, value){
      value = !!value ? value : that.progress;
      if (that.showPercent){
        $(self).find('span.percent-text').html(Math.floor(value*100)+"%");
      }
    }
  },
  methods:{
    convertedProgress(progress) {
      return progress/100;
    },
    updateProgress(value){
      if ($.type(value) === "number") {
        $('#' + this.id).circleProgress('value',this.convertedProgress(value));
      }else{
        console.error("Passed Invalid Value. Number Expected. (Hint: use parseInt())")
      }
    }
  }
}
</script>

<style scoped>
.circle {
  position: relative;
  display: inline-block;
}
.circle-percent-text-body {
  position: absolute;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.percent-text {
  font-weight: bold;
}
</style>
