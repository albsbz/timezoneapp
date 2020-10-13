<template>
  <div class="hello">
    <h1 class="title is-1">Appointment scheduler</h1>
    <div class="wrapper">

      <div class="column box">
        <p class="title is-3">My Timezone</p>
        <div class="field">
          <label class="label">Select your Timezone</label>
          <div class="control" v-if="ifShowYouControl">
            <div class="select">
              <select>
                <option v-for="timezone in timezones" 
                        value="timezone" 
                        v-bind:key="timezone">
                        UTC {{timezone}}
                </option>
              </select>
            </div>
          </div>
          <label class="label">Your time</label>
          <div class="control">
            <input class="input is-hovered time" type="text" placeholder="Time" maxlength="5" 
                  :value="myTimeShow" 
                  @keydown.prevent="updateMyTime"
                  @focus="ifShowPartnerControl=false">
          </div>
        </div> 
      </div>

      <div class="column box">
        <p class="title is-3">Partner Timezone</p>
        <div class="field">
          <label class="label">Select partner Timezone</label>
          <div class="control">
            <div class="select">
              <select>
                <option v-for="timezone in timezones" 
                        value="timezone" 
                        v-bind:key="timezone">
                        UTC {{timezone}}
                </option>
              </select>
            </div>
          </div>
          <label class="label">Partner time</label>
          <div class="control" v-if="ifShowPartnerControl">
            <input class="input is-hovered time" type="text" placeholder="Time" maxlength="5" 
                  :value="partnerTimeShow" 
                  @keydown.prevent="updatePartnerTime">
          </div>
        </div>


      </div>

    </div>

    <div class="field is-grouped control-wrapper">
      <div class="control">
        <button class="button is-link">Schedule</button>
      </div>
      <div class="control">
        <button class="button is-link is-light" @click="clearForm">Clear</button>
      </div>
    </div>
  </div>
  


</template>

<script>
export default {
  name: 'TimezoneApp',
  data: function () {return {
      timezones: [-11,-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
      myTime: [0,0,0,0],
      partnerTime: [0,0,0,0],
      ifShowPartnerControl: true,
      ifShowYouControl: true,
    }
  },
  computed: {
    myTimeShow: function(){
      let newArr=[...this.myTime];
      newArr.splice(2, 0, ':');
      return newArr.join('')
    },
    partnerTimeShow: function(){
      let newArr=[...this.partnerTime];
      newArr.splice(2, 0, ':');
      return newArr.join('')
    }
    
  },
  methods: {
    updateMyTime: function(e){
      if ((e.keyCode>47&&e.keyCode<58)||(e.keyCode>95&&e.keyCode<106)) {
        this.myTime.push(e.key);
        this.myTime=this.myTime.slice(-4);
      }
    },
    clearForm: function(){
      this.myTime=[0,0,0,0];
      this.partnerTime=[0,0,0,0];
      this.ifShowPartnerControl=true;
      this.ifShowYouControl=true;
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
