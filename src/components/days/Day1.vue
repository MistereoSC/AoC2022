<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/1" target="_blank">Day 1: Calorie Counting</a>      
    </div>
    <div>
      <textarea v-model="inputMessage" rows="12" cols="50" placeholder="Input"></textarea>
    </div>
    <div>
      <button @click="calculateOutput">Calculate</button>
      <input v-model="outputMessage" placeholder="Output"/>
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
      inputMessage: ""
    }
  },
  methods: {
    calculateOutput() {
      //Split input into sets at empty lines
      var set = this.inputMessage.split(/\n\s*\n/)
      var intermediateMax = []
      for(let i=0; i<set.length; i++){
        //Split sets further at linebreaks
        set[i]=set[i].split(/\n/)
        //Convert each string to number
        for(let j=0;j<set[i].length;j++){
          set[i][j]=parseInt(set[i][j])
        }
        //Sum each set
        intermediateMax[i]=set[i].reduce((acc,c)=>acc+c)
      }
      //Return the largest sum of all sets
      this.outputMessage=Math.max(...intermediateMax)
    }
  }
}
</script>