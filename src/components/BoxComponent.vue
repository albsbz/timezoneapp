<template>
  <div class="column box">
    <p class="title is-3">{{name}}</p>
    <div class="field">
      <label class="label">Select partner Timezone</label>
      <div class="control">
        <div class="select" >
          <select 
            @change="utcChange($event)"
          >
            <option v-for="(timezone, index) in timezones" 
                    :value="index" 
                    :key="timezone.hours+'.'+timezone.minutes"
                    :selected="index===14"
                    >
                    UTC {{timezone.hours}}.{{timezone.minutes}}
            </option>
          </select>
        </div>
      </div>
      <label class="label">Time</label>
      <div class="control" >
        <time-input
          @timeChange="timeChange"
          v-show="ifShowInput"
        />
        <div v-show="!ifShowInput">{{appointedTime}}</div>
      </div>

      <div >
        
      </div>
    </div>


  </div>
</template>

<script>
import TimeInput from './TimeInput.vue'

export default {
  name: 'BoxComponent',
  components: {
    TimeInput,
    
  },
  props:{
    name:String,
    id:Number,
    ifShowInput:Boolean,
    appointedTime:String
  },
  data: function () {return {
      timezones: [
        {hours: -12, minutes:0},
        {hours: -11, minutes:0},
        {hours: -10, minutes:0},
        {hours: -9, minutes:0},
        {hours: -9, minutes:30},
        {hours: -8, minutes:0},
        {hours: -7, minutes:0},
        {hours: -6, minutes:0},
        {hours: -5, minutes:0},
        {hours: -4, minutes:0},
        {hours: -3, minutes:0},
        {hours: -3, minutes:30},
        {hours: -2, minutes:0},
        {hours: -1, minutes:0},
        {hours: 0, minutes:0},
        {hours: 1, minutes:0},
        {hours: 2, minutes:0},
        {hours: 3, minutes:0},
        {hours: 3, minutes:30},
        {hours: 4, minutes:0},
        {hours: 4, minutes:30},
        {hours: 5, minutes:0},
        {hours: 5, minutes:30},
        {hours: 5, minutes:45},
        {hours: 6, minutes:0},
        {hours: 6, minutes:30},
        {hours: 7, minutes:0},
        {hours: 8, minutes:0},
        {hours: 8, minutes:45},
        {hours: 9, minutes:0},
        {hours: 9, minutes:30},
        {hours: 10, minutes:0},
        {hours: 10, minutes:30},
        {hours: 11, minutes:0},
        {hours: 12, minutes:0},
        {hours: 12, minutes:45},
        {hours: 13, minutes:0},
        {hours: 14, minutes:0},
      ],


    }
  },
  computed: {
   
    
  },
  methods: {
    utcChange: function(e){
      this.$emit('utcChange', {
        newUtc: this.timezones[e.target.value],
        id: this.id
      })
    },
    timeChange: function(time){
      this.$emit('timeChange', {
        time,
        id: this.id
      })
    },
    isTimeValid: function(){
      let myTime=this.$refs.myTimer.giveTime();
      let partnerTime=this.$refs.partnerTimer.giveTime();
      if (myTime===false) {
        this.$refs.myTimer.$el.focus();
        this.scheduleDisabled=true;
        return false
      }
      else if (partnerTime===false) {
        this.$refs.partnerTimer.$el.focus();
        this.scheduleDisabled=true;
        return false
      }
      else {
        this.scheduleDisabled=false;
        return {myTime, partnerTime}
      }
    }
  }
}
</script>

<style  scoped>
.time{
  width:65px;
}
</style>