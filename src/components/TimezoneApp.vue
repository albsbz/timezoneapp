<template>
  <div class="hello">
    <h1 class="title is-1">Appointment scheduler</h1>
    <div class="wrapper">

      <div class="column box">
        <p class="title is-3">My Time</p>
        <div class="field">
          <label class="label">Select your Timezone</label>
          <div class="control">
            <div class="select">
              <select v-model="myTimezone">
                <option v-for="timezone in timezones" 
                        :value="timezone" 
                        :key="timezone.hours+'.'+timezone.minutes">
                        UTC {{timezone.hours}}:{{timezone.minutes=='0'?'00':timezone.minutes}}
                </option>
              </select>
            </div>
          </div>
          <label class="label">Your time</label>
          <div class="control" v-show="ifShowYouControl"> 
            <time-input
              ref="myTimer" 
              @focus.native="ifShowPartnerControl=false"
              @keyup.native="isTimeValid"
            />
          </div>
          <div v-show="!ifShowYouControl">
            {{myAppointedTime?myAppointedTime.hours:''}}:{{myAppointedTime?(myAppointedTime.minutes=='0'?'00':myAppointedTime.minutes):''}}
          </div>
        </div> 
      </div>

      <div class="column box">
        <p class="title is-3">Partner Time</p>
        <div class="field">
          <label class="label">Select partner Timezone</label>
          <div class="control">
            <div class="select" >
              <select v-model="partnerTimezone">
                <option v-for="timezone in timezones" 
                        :value="timezone" 
                        :key="timezone.hours+'.'+timezone.minutes">
                        UTC {{timezone.hours}}.{{timezone.minutes=='0'?'00':timezone.minutes}}
                </option>
              </select>
            </div>
          </div>
          <label class="label">Partner time</label>
          <div class="control" v-show="ifShowPartnerControl">
            <time-input
              ref="partnerTimer"
              @focus.native="ifShowYouControl=false"
              @keyup.native="isTimeValid"
            />
          </div>
          <div v-show="!ifShowPartnerControl">
            {{partnerAppointedTime?partnerAppointedTime.hours:''}}:{{partnerAppointedTime?(partnerAppointedTime.minutes=='0'?'00':partnerAppointedTime.minutes):''}}
          </div>
        </div>


      </div>

    </div>

    <div class="field is-grouped control-wrapper">
      <div class="control">
        <button class="button is-link" @click="schedule" :disabled="scheduleDisabled">Schedule</button>
      </div>
      <div class="control">
        <button class="button is-link is-light" @click="clearForm">Clear</button>
      </div>
    </div>
  </div>
  


</template>

<script>
import TimeInput from './TimeInput.vue'

export default {
  name: 'TimezoneApp',
  components: {
    TimeInput
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

      myTimezone: {hours: 0, minutes:0},
      partnerTimezone: {hours: 0, minutes:0},
      myAppointedTime: undefined,
      partnerAppointedTime: undefined,
      ifShowPartnerControl: true,
      ifShowYouControl: true,
      scheduleDisabled:false,
    }
  },
  computed: {
   
    
  },
  methods: {
   
    clearForm: function(){
      this.ifShowPartnerControl=true;
      this.ifShowYouControl=true;
      this.$refs.myTimer.clearInput();
      this.$refs.partnerTimer.clearInput();
      this.scheduleDisabled=false; 
      this.myAppointedTime= undefined;
      this.partnerAppointedTime=undefined;     
    },
    schedule: function(){
      let time=this.isTimeValid();
      if (time){
        if (!isNaN(time.myTime.hours)){
          let newTime=time.myTime.hours*60+time.myTime.minutes-(this.myTimezone.hours*60+this.myTimezone.minutes)+(this.partnerTimezone.hours*60+this.partnerTimezone.minutes)

          this.partnerAppointedTime={
            hours: (24+(newTime-newTime%60)/60)%24,
            minutes: newTime%60
          }
          this.myAppointedTime= undefined;
        } else {
          let newTime=time.partnerTime.hours*60+time.partnerTime.minutes-(this.partnerTimezone.hours*60+this.partnerTimezone.minutes)+(this.myTimezone.hours*60+this.myTimezone.minutes)

          this.myAppointedTime={
            hours: (24+(newTime-newTime%60)/60)%24,
            minutes: newTime%60
          }
          this.partnerAppointedTime= undefined;
        }
      }

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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapper {
  width: 80vw;
  margin: auto;
  display: flex;
  justify-content: center;
}
.column {
  margin: 0 10px;
  width: 50%;
}
.control-wrapper {
  width: 200px;
  margin:10px auto;
}
.time{
  width:65px;
}
</style>
