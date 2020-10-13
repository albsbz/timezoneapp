<template>
  <div class="hello">
    <h1 class="title is-1">Appointment scheduler</h1>
    <div class="wrapper">

      <div class="column box">
        <p class="title is-3">My Timezone</p>
        <div class="field">
          <label class="label">Select your Timezone</label>
          <div class="control">
            <div class="select">
              <select v-model="myTimezone">
                <option v-for="timezone in timezones" 
                        :value="timezone" 
                        :key="timezone">
                        UTC {{timezone}}
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
            {{myAppointedTime?myAppointedTime.hours:''}}:{{myAppointedTime?myAppointedTime.minutes:''}}
          </div>
        </div> 
      </div>

      <div class="column box">
        <p class="title is-3">Partner Timezone</p>
        <div class="field">
          <label class="label">Select partner Timezone</label>
          <div class="control">
            <div class="select" >
              <select v-model="partnerTimezone">
                <option v-for="timezone in timezones" 
                        :value="timezone" 
                        :key="timezone">
                        UTC {{timezone}}
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
            {{partnerAppointedTime?partnerAppointedTime.hours:''}}:{{partnerAppointedTime?partnerAppointedTime.minutes:''}}
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
      timezones: [-11,-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
      // myTime: [0,0,0,0],
      // partnerTime: [0,0,0,0],
      myTimezone: 0,
      partnerTimezone: 0,
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
          this.partnerAppointedTime={
            hours: (time.myTime.hours-this.myTimezone+this.partnerTimezone)%24,
            minutes: time.myTime.minutes
          }
          this.myAppointedTime= undefined;
        } else {
          this.myAppointedTime={
            hours: (time.partnerTime.hours-this.partnerTimezone+this.myTimezone)%24,
            minutes: time.partnerTime.minutes
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
        console.log({myTime, partnerTime})
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
