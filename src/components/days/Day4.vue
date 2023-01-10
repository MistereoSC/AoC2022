<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/4" target="_blank">Day 4: Camp Cleanup</a>      
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
      //Split input at linebreaks
      var splittedInput = this.inputMessage.split(/\n/)
      var sumOfMatches = 0
      for(let i = 0;i<splittedInput.length;i++){
        var pair = (splittedInput[i].split(','))
        var leftStart = parseInt(pair[0].split('-')[0])
        var leftEnd = parseInt(pair[0].split('-')[1])
        var rightStart = parseInt(pair[1].split('-')[0])
        var rightEnd = parseInt(pair[1].split('-')[1])
        //check if left is completely contained in right, and vice versa
        if(leftStart<=rightStart && leftEnd>=rightEnd) {sumOfMatches++}
        else if(rightStart<=leftStart && rightEnd>=leftEnd) {sumOfMatches++}
      }
      this.outputMessage=sumOfMatches
    },
    calculateOutput2() {
      //split input at linebreaks
      var splittedInput = this.inputMessage.split(/\n/)
      var sumOfMatches = 0
      //split each input in pairs, and each pair into start and end
      for(let i = 0;i<splittedInput.length;i++){
        var pair = (splittedInput[i].split(','))
        var leftStart = parseInt(pair[0].split('-')[0])
        var leftEnd = parseInt(pair[0].split('-')[1])
        var rightStart = parseInt(pair[1].split('-')[0])
        var rightEnd = parseInt(pair[1].split('-')[1])
        //Check if any numbers overlap
        if(leftEnd>=rightStart && leftStart<=rightEnd) {sumOfMatches++}
      }
      this.outputMessage2=sumOfMatches
    }
  }
}
</script>