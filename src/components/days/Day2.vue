<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/2" target="_blank">Day 2: Rock Paper Scissors</a>      
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
      var partialScores = []
      //Calculate score for each input line
      for(let i=0;i<splittedInput.length;i++){
        var c = 0
        switch(splittedInput[i]){
          case "A X":
            c=1+3
            break;
          case "A Y":
            c=2+6
            break;
          case "A Z":
            c=3+0
            break;
          case "B X":
            c=1+0
            break;
          case "B Y":
            c=2+3
            break;
          case "B Z":
            c=3+6
            break;
          case "C X":
            c=1+6
            break;
          case "C Y":
            c=2+0
            break;
          case "C Z":
            c=3+3
            break;
        }
      partialScores[i]=c
      }
      //Sum all partial scores
      this.outputMessage=partialScores.reduce((acc,c)=>acc+c)
    },
    calculateOutput2() {
      //split input at linebreaks
      var splittedInput = this.inputMessage.split(/\n/)
      var partialScores = []
      //Calculate score for each input line
      for(let i=0;i<splittedInput.length;i++){
        var c = 0
        switch(splittedInput[i]){
          case "A X":
            c=0+3
            break;
          case "A Y":
            c=3+1
            break;
          case "A Z":
            c=6+2
            break;
          case "B X":
            c=0+1
            break;
          case "B Y":
            c=3+2
            break;
          case "B Z":
            c=6+3
            break;
          case "C X":
            c=0+2
            break;
          case "C Y":
            c=3+3
            break;
          case "C Z":
            c=6+1
            break;
        }
      partialScores[i]=c
      }
      //Sum all partial scores
      this.outputMessage2=partialScores.reduce((acc,c)=>acc+c)
    }
  }
}
</script>