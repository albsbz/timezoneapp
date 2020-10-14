<template>
    <input class="input is-hovered time" type="text" placeholder="Time" maxlength="5" 
        :value="timeShow" 
        @keydown.prevent="updateTime"
        @clear="clearInput"
        >
</template>

<script>
export default {
  name: 'TimeInput',
  data: function () {return {
      time: [undefined,undefined,undefined,undefined]
    }
  },
  computed: {
    timeShow: function(){
      let newArr=[...this.time];
      newArr.splice(2, 0, ':');
      return newArr.join('')
    },    
  },
  methods: {
    updateTime: function(e){
      if ((e.keyCode>47&&e.keyCode<58)||(e.keyCode>95&&e.keyCode<106)) {
        this.time.push(e.key);
        this.time=this.time.slice(-4);
      }
    if ((e.keyCode===8)) {
        this.time.pop();
      }
    },
    giveTime: function(){
        if (this.time[0]===undefined) {this.time[0]=0}
        if (this.time[3]===undefined) {this.time[3]=0}
        let hours = Number(this.time[0]+this.time[1]);
        let minutes = Number(this.time[2]+this.time[3]);
        return (hours>24||minutes>60)?false:{hours, minutes}      
    },
    clearInput: function(){
      this.time=[undefined,undefined,undefined,undefined];
    }
    
  }
}
</script>

<style scoped>
    .time{
        text-align: center;
    }
</style>