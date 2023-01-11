<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/6" target="_blank">Day 6: Tuning Trouble</a>      
    </div>
    <div>
      <textarea v-model="inputMessage" rows="12" cols="50" placeholder="Input"></textarea>
    </div>
    <div>
      <button @click="calculateOutput">Calculate</button>
      <input v-model="outputMessage" placeholder="Output Q1"/>
    </div>
    <div>
      <button @click="calculateOutput2">Calculate</button>
      <input v-model="outputMessage2" placeholder="Output Q2"/>
    </div>

    
  </div>
</template>

<style scoped>

</style>

<script>
export default{
  data(){
    return {
      outputMessage: "",
      outputMessage2: "",
      inputMessage: ""
    }
  },
  methods: {
    calculateOutput() {
      //convert input to char array
      var charArray = Array.from(this.inputMessage)
      //iterate through the entire array
      for(let i=0;i<charArray.length-3;i++){
        //compare 4 characters and remove duplicates
        var t = charArray.slice(i,i+4)
          .reduce(function(acc,c){
            if(!acc.includes(c)) {acc.push(c)}
            return acc
          },[])
        //if no duplicates are present, the index is found
        if(t.length==4){
          console.log(t)
          this.outputMessage=i+4
          return
        }
      }
    },
    calculateOutput2() {
      var charArray = Array.from(this.inputMessage)
      for(let i=0;i<charArray.length-13;i++){
        var t = charArray.slice(i,i+14)
          .reduce(function(acc,c){
            if(!acc.includes(c)) {acc.push(c)}
            return acc
          },[])
        if(t.length==14){
          console.log(t)
          this.outputMessage2=i+14
          return
        }
      }
    }
  }
}
</script>