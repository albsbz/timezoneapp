<template>
  <div class="hello">
    <h1 class="title is-1">Appointment scheduler</h1>
    
    <div class="wrapper">
        <box-component 
            :name="myTime"
            :id=0
            @utcChange="utcChange"
            @timeChange="timeChange"
            :ifShowInput="ifShowYouControl"
            :appointedTime="myAppointedTime"        
        />
        <box-component 
            :name="PartnerTime"
            :id=1
            @utcChange="utcChange" 
            @timeChange="timeChange" 
            :ifShowInput="ifShowPartnerControl"
            :appointedTime="partnerAppointedTime"        
        />
    </div>

    <div class="field is-grouped control-wrapper">
      <div class="control">
        <button class="button is-link is-light" @click="clearForm">Clear</button>
      </div>
    </div>
  </div>

</template>

<script>
import BoxComponent from './BoxComponent.vue'

export default {
    name: 'TimezoneApp',
    components: {
        BoxComponent
    },
    data: function(){return{
        myTime: 'My Time',
        PartnerTime: 'Partner Time',
        showBox: {
            name: undefined,
            utc: undefined,
            time: undefined
        },
        payload: [{
            id:0,
            time:{ 
                hr:0,
                min:0
            },
            utc:{ 
                hours:0,
                minutes:0
            },
        },{
            id:1,
            time:{ 
                hr:0,
                min:0
            },
            utc:{ 
                hours:0,
                minutes:0
            },
        }
        ],
        lastUpdated:0,
        ifShowPartnerControl: true,
        ifShowYouControl: true,
        myAppointedTime: undefined,
        partnerAppointedTime: undefined,
    }},
    methods: {
        utcChange: function({newUtc, id}){
            this.payload[id]={...this.payload[id], id, utc:newUtc}
            this.schedule(this.lastUpdated);

        },
        timeChange: function({time, id}){
            this.payload[id]={...this.payload[id],id, time}
            this.lastUpdated=id;
            this.schedule(id);
        },
        clearForm: function(){
        //   this.ifShowPartnerControl=true;
        //   this.ifShowYouControl=true;
        //   this.$refs.myTimer.clearInput();
        //   this.$refs.partnerTimer.clearInput();
        //   this.scheduleDisabled=false; 
        //   this.myAppointedTime= undefined;
        //   this.partnerAppointedTime=undefined;     
        },
        schedule: function(id){
            const newTime = (t, tz1, tz2) => {
                let newTimeInSec=t.hr*60+t.min-(tz1.hours*60+tz1.minutes)+(tz2.hours*60+tz2.minutes);
                return (24+(newTimeInSec-newTimeInSec%60)/60)%24+":"+newTimeInSec%60
            }
            
            if (id===0){
                this.ifShowPartnerControl=false;
                this.ifShowYouControl=true;
                this.partnerAppointedTime=newTime(this.payload[0].time, this.payload[0].utc, this.payload[1].utc)
            } else {
                this.ifShowYouControl=false;
                this.ifShowPartnerControl=true;
                this.myAppointedTime=newTime(this.payload[1].time, this.payload[1].utc, this.payload[0].utc)
            }
        }
    },  
   
}



</script>

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

</style>