<template>
  <div>
      <input class="input is-hovered time" type="number" placeholder="Hr" min="0" max="23" maxlength="2"
      v-model="hr" 
      @keyup="updateTime"
      :class="[{'is-danger':isHrDanger}]"
    >:
    <input class="input is-hovered time" type="number" placeholder="Min" min="0" max="59" maxlength="2"
      v-model="min"
      @keyup="updateTime"
      :class="[{'is-danger':isMinDanger}]"
    >
  </div>
 
</template>

<script>
export default {
  name: 'TimeInput',
  data: function () {return {
      hr: 0,
      min:0,
      isHrDanger: false,
      isMinDanger: false
    }
  },
  computed: {
    // time: function(){
    //   return{
    //     h
    //   }
    // },    
  },
  methods: {
    updateTime: function(){
      this.isMinDanger=true;
      this.isHrDanger=true;

      let hr=parseInt(this.hr);
      let min=parseInt(this.min);
      if (hr>=0&&hr<=23) {
        this.isHrDanger=false;
      }
      if(min>=0&&min<=59){
        this.isMinDanger=false;
      }

      if (!this.isHrDanger && !this.isMinDanger) {
        this.$emit('timeChange', {hr, min});
      }
    },
    giveTime: function(){
        let hours = Number(this.time[0]+this.time[1]);
        let minutes = Number(this.time[2]+this.time[3]);
        return (hours>24||minutes>60)?false:{hours, minutes}      
    },
    clearInput: function(){
      this.hr=0;
      this.min=0;
    }
    
  }
}
</script>

<style scoped>
    .time{
        text-align: center;
        width: 50px;
        font: 400 13.3333px Arial;
        vertical-align: middle;


    }
    .time::-webkit-outer-spin-button,
    .time::-webkit-inner-spin-button {
        /* display: none; <- Crashes Chrome on hover */
        -webkit-appearance: none;
        margin: 0; /* <-- Apparently some margin are still there even though it's hidden */
    }

    .time[type=number] {
        -moz-appearance:textfield; /* Firefox */
    }
    
</style>