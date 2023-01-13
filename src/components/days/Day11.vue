<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/11" target="_blank">Day 11: Monkey in the Middle</a>      
    </div>
    <div>
      <textarea v-model="inputMessage" rows="12" cols="50" placeholder="Input"></textarea>
    </div>
    <div>
      <button @click="setup(1)">Calculate</button>
      <input v-model="outputMessage" placeholder="Output"/>
    </div>
    <div>
      <button @click="setup(2)">Calculate</button>
      <input v-model="outputMessage2" placeholder="Output"/>
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
      monkeys: [],
      commonDivisor: 96577
    }
  },
  methods: {
    setup(param) {
      var set = this.inputMessage.split(/\n\s*\n/)
      this.monkeys=[]
      var divisor = 1
      for(let i=0;i<set.length;i++){
        this.monkeys.push({
          items: [],
          opSgn: "",
          opVal: "",
          tst: 0,
          trueTo: 0,
          falseTo: 0,
          inspections: 0
        })
        var lns = set[i].split(/\n/)
        var t1 = lns[1].split(": ")[1].split(", ")
        for(let k=0;k<t1.length;k++){this.monkeys[i].items.push(parseInt(t1[k]))}
        t1 = lns[2].split(": ")[1].split(" ")
        this.monkeys[i].opSgn = t1[3]
        this.monkeys[i].opVal = t1[4]
        t1 = lns[3].split(": ")[1].split(" ")
        this.monkeys[i].tst = t1[2]
        t1 = lns[4].split(": ")[1].split(" ")
        this.monkeys[i].trueTo = t1[3]
        t1 = lns[5].split(": ")[1].split(" ")
        this.monkeys[i].falseTo = t1[3]

        divisor*=this.monkeys[i].tst
      }
      this.commonDivisor=divisor
      this.play(param)
    },
    play(part) {
      var MAX_ROUNDS
      if(part==2){MAX_ROUNDS=10000}
      else{MAX_ROUNDS=20}
      var round = 0
      for(round;round<MAX_ROUNDS;round++){
        for(let m=0;m<this.monkeys.length;m++){
          for(let i=0;i<this.monkeys[m].items.length;i++){
            var worryLevel = this.monkeys[m].items[i]
            if(this.monkeys[m].opSgn=="*"){
              if(this.monkeys[m].opVal=="old"){worryLevel*=worryLevel}
              else{worryLevel*=parseInt(this.monkeys[m].opVal)}
            }
            else if(this.monkeys[m].opSgn=="+"){
              if(this.monkeys[m].opVal=="old"){worryLevel+=worryLevel}
              else{worryLevel+=parseInt(this.monkeys[m].opVal)}
            }
            if(part==1){worryLevel=Math.floor(worryLevel/3)}
            else(worryLevel=worryLevel%this.commonDivisor)
            if(worryLevel%this.monkeys[m].tst==0){this.monkeys[this.monkeys[m].trueTo].items.push(worryLevel)}
            else{this.monkeys[this.monkeys[m].falseTo].items.push(worryLevel)}
            this.monkeys[m].inspections++
          }
          this.monkeys[m].items = []
        }
      }

      var first = -1
      var second = -1
      for(let c=0;c<this.monkeys.length;c++){
        if(this.monkeys[c].inspections>first){
          second = first
          first = this.monkeys[c].inspections
        }
        else if(this.monkeys[c].inspections>second){
          second = this.monkeys[c].inspections
        }
      }
      if(part==1){this.outputMessage=first*second}
      else{this.outputMessage2=first*second}
    }
  }
}
</script>