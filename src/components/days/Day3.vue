<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/3" target="_blank">Day 3: Rucksack Reorganization</a>      
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
      var splittedInput = this.inputMessage.split(/\n/)
      var sumOfPriorites = 0
      for(let i=0;i<splittedInput.length;i++){
        //split input string in left and right half and store as char array
        var left = Array.from(splittedInput[i].substring(0,splittedInput[i].length/2))
        var right = Array.from(splittedInput[i].substring(splittedInput[i].length/2, splittedInput[i].length))
        //create array that only contains chars present in both left and right
        var out = left.filter(c => right.includes(c))
          //remove duplicates from char array
          .reduce(function(acc,c){
            if(!acc.includes(c)) {acc.push(c)}
            return acc
          },[])
        //calculate priorities (a-z=1-26,A-Z=27-52)
        for(let i=0;i<out.length;i++){
          var c = out[i].charCodeAt(0)
          if(c>=97){sumOfPriorites+=c-96}
          else{sumOfPriorites+=c-38}
        }
      }
      this.outputMessage=sumOfPriorites
    },
    calculateOutput2() {
      var splittedInput = this.inputMessage.split(/\n/)
      var sumOfPriorites = 0
      //select inputs in pairs of 3
      for(let i=0;i<splittedInput.length-2;i+=3){
        var in1 = Array.from(splittedInput[i])
        var in2 = Array.from(splittedInput[i+1])
        var in3 = Array.from(splittedInput[i+2])
        //find chars present in all 3 inputs
        var out = in1.filter(char => in2.includes(char)&&in3.includes(char))
          //remove duplicates
          .reduce(function(acc,c){
              if(!acc.includes(c)) {acc.push(c)}
              return acc
          },[])
        //calculate priorities (a-z=1-26,A-Z=27-52)
        for(let i=0;i<out.length;i++){
          var c = out[i].charCodeAt(0)
          if(c>=97){sumOfPriorites+=c-96}
          else{sumOfPriorites+=c-38}
        }
      }
      this.outputMessage2=sumOfPriorites
    }
  }
}
</script>