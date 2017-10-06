<template>
  <div>
    <div :id="id" class="circle">
      <span id="percent-text"></span>
    </div>
  </div>
</template>

<script>
 var $ = require('jquery');
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
    innerText:{
      type : String
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
      console.log(value);
      if ((!that.innerText) && that.showPercent)
        $(self).find('span').html(Math.floor(value*100)+"%");
      else {
        $(self).find('span').html(that.innerText);
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
