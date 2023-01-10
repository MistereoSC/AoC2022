<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/5" target="_blank">Day 5: Supply Stacks</a>      
    </div>
    <div>
      <textarea v-model="inputMessage" rows="12" cols="50" placeholder="Input"></textarea>
    </div>
    <div>
      <button @click="stepCalculation">Step</button>
      <button @click="calculateOutput">Solve</button>
      <input v-model="outputMessage" placeholder="Output Q1"/>
      <button @click="setup">Reset</button>
    </div>
    <div>
      <textarea v-model="outputStacks" rows="12" cols="50" placeholder="Output Visual Q1"></textarea>
    </div>
    <div>
      <button @click="stepCalculation">Step</button>
      <button @click="calculateOutput">Solve</button>
      <input v-model="outputMessage2" placeholder="Output Q2"/>
      <button @click="calculateOutput">Reset</button>
    </div>
    <div>
      <textarea v-model="outputStacks2" rows="12" cols="50" placeholder="Output Visual Q2"></textarea>
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
      inputMessage: "",
      outputStacks: "",
      outputStacks2: "",
      currentStep: -1,
      stacks: [],
      stacks2: [],
      moves: []
    }
  },
  methods: {
    setup() {
      this.outputMessage=this.outputMessage2=this.outputStacks=this.outputStacks2=""
      //get stack input
      var stackString = this.inputMessage.split(/\n\s*\n/)[0].split(/\n/)
      //initialize stack array with empty arrays
      this.stacks = new Array(Math.ceil(stackString[0].length/4))
      for(let i=0;i<this.stacks.length;i++){this.stacks[i]=new Array()}

      for(let i=stackString.length-2;i>=0;i--){
        //split each line into its blocks
        var t = stackString[i].match(/\[.\]|(    )/g)
        for(let j=0;j<t.length;j++){
          //populate array
          if(t[j].substring(1,2)!==' '){this.stacks[j].push(t[j].substring(1,2))}
        }
      }
      //clone stacks
      this.stacks2 = JSON.parse(JSON.stringify(this.stacks))
      //split move strings into objects
      var moveList = this.inputMessage.split(/\n\s*\n/)[1].split(/\n/)
      this.moves = new Array(moveList.length)
      for(let i=0;i<this.moves.length;i++){
        var t = moveList[i].split(' ')
        this.moves[i]={
          from: t[3]-1,
          to: t[5]-1,
          amount: t[1]
        }
      }
      //set step to 0 for stepper to signal that initial input reading is complete
      this.currentStep=0
    },
    stepCalculation() {
      //Return if all moves are stepped through
      if(this.currentStep>=this.moves.length){return}
      //Read input if its the first step
      if(this.currentStep<=0){this.setup()}

      //Pop n items from location and push to destination for Q1 stack
      for(let i=0;i<this.moves[this.currentStep].amount;i++){
        var t = this.stacks[this.moves[this.currentStep].from].pop()
        this.stacks[this.moves[this.currentStep].to].push(t)
      }
      //Pop n items from location and push to destination for Q2 stack
      var s = new Array()
      for(let i=0;i<this.moves[this.currentStep].amount;i++){
        s.push(this.stacks2[this.moves[this.currentStep].from].pop())
      }
      for(let i=0;i<this.moves[this.currentStep].amount;i++){
        var t = s.pop()
        this.stacks2[this.moves[this.currentStep].to].push(t)
      }

      //Create output string (letter of the last item of each row) for Q1
      var out = ""
      for(let i=0;i<this.stacks.length;i++){
        if(this.stacks[i][this.stacks[i].length-1]!==undefined){
          out+=this.stacks[i][this.stacks[i].length-1]
        }else{out+="  "}
      }
      this.outputMessage = out
      //Create output string (letter of the last item of each row) for Q2
      out = ""
      for(let i=0;i<this.stacks2.length;i++){
        if(this.stacks2[i][this.stacks2[i].length-1]!==undefined){
          out+=this.stacks2[i][this.stacks2[i].length-1]
        }else{out+="  "}
      }
      this.outputMessage2 = out


      //Fill visual output and increase counter
      this.outputStacks = this.textifyStacks(this.stacks)
      this.outputStacks2 = this.textifyStacks(this.stacks2)
      this.currentStep++
    },
    calculateOutput() {
      for(let i=this.currentStep;i<=this.moves.length;i++){
        this.stepCalculation()
      }
    },
    textifyStacks(stack) {
      //convert stacks array to a string
      var out = ""
      //First line shows current step, and if the program is done
      if(this.currentStep>=this.moves.length-1){out+="DONE! Steps: "+this.currentStep+"\n"}
      else{out+="Step "+this.currentStep+": Move "+this.moves[this.currentStep].amount+" from "+this.moves[this.currentStep].from+" to "+this.moves[this.currentStep].to+"\n"}
      //Calculate the highest stack
      var maxStackSize = Math.max(...stack.map(c=>c.length))
      //Convert each row to string
      for(let i=maxStackSize-1;i>=0;i--){
        for(let j=0;j<stack.length;j++){
          if(stack[j][i]===undefined){out+="    "}
          else{out+="["+stack[j][i]+"] "}
        }
        out+='\n'
      }
      //Add row numbers to bottom
      for(let i=0;i<stack.length;i++){
        out+=" "+i+"  "
      }
      out+="\n"
      return out
    }
  }
}
</script>