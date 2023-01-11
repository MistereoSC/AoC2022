<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/10" target="_blank">Day 10: Cathode-Ray Tube</a>      
    </div>
    <div>
      <textarea v-model="inputMessage" rows="12" cols="50" placeholder="Input"></textarea>
    </div>
    <div>
      <button @click="calculateOutput">Calculate</button>
      <input v-model="outputMessage" placeholder="Output"/>
    </div>
    <div>
      <textarea v-model="outputVisual" rows="12" cols="50" placeholder="Output Visual"></textarea>
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
      outputVisual: "",
      inputMessage: ""
    }
  },
  methods: {
    calculateOutput() {
      var ops = this.inputMessage.split(/\n/)
      for(let i=0;i<ops.length;i++){
        ops[i]=ops[i].split(' ')
        if(ops[i][1]!==undefined){ops[i][1]=parseInt(ops[i][1])}
      }

      var cycle = 0
      var opCount = 0
      var acc = 1
      var out = 0
      var scan = ""

      while(opCount<ops.length){
        if(cycle%40==acc||cycle%40==acc-1||cycle%40==acc+1){scan+="█"}else{scan+="."}

        cycle++
        if(cycle%40==0){scan+="\n"}
        if(cycle%40==20){out+=cycle*acc}
        if(ops[opCount][0]=="noop"){opCount++;}
        else {
          if(cycle%40==acc||cycle%40==acc-1||cycle%40==acc+1){scan+="█"}else{scan+="."}
          cycle++
          if(cycle%40==0){scan+="\n"}
          if(cycle%40==20){out+=cycle*acc}
          acc+=ops[opCount][1]
          opCount++
        }
      }
      this.outputMessage=out
      this.outputVisual=scan
    }
  }
}
</script>