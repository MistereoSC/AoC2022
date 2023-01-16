<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/14" target="_blank">Day 14: Regolith Reservoir</a>      
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
    <div>
      <textarea v-model="outputVisual" rows="50" cols="400" placeholder="Output Visual"></textarea>
    </div>

    
  </div>
</template>

<style scoped>
textarea{
  overflow-x: scroll;
}
</style>

<script>
export default{
  data(){
    return {
      outputMessage: "",
      outputMessage2: "",
      inputMessage: "",
      outputVisual: ""
    }
  },
  methods: {
    calculateOutput() {
      var line = this.inputMessage.split(/\n/)
      var X_MAX=0
      var Y_MAX=0
      //Read Input and calculate map size
      for(let idx=0;idx<line.length;idx++){
        line[idx] = line[idx].split(" -> ")
        for(let c=0;c<line[idx].length;c++){
          var t = line[idx][c].split(",")
          line[idx][c] = {x:parseInt(t[0]), y:parseInt(t[1])}
          //find the highest X and Y coordinates
          if(parseInt(t[0])>X_MAX){X_MAX=parseInt(t[0])}
          if(parseInt(t[1])>Y_MAX){Y_MAX=parseInt(t[1])}
        }
      }
      //Build empty map
      Y_MAX+=1
      X_MAX*=2
      var map = new Array(Y_MAX)
      for(let y=0;y<Y_MAX;y++){
        map[y] = new Array(X_MAX)
        for(let x=0;x<X_MAX;x++){
          map[y][x]="."
        }
      }
      //Populate map
      for(let ln=0;ln<line.length;ln++){
        for(let op=1;op<line[ln].length;op++){
          if(line[ln][op].x!=line[ln][op-1].x){
            if(line[ln][op].x>line[ln][op-1].x){
              //RIGHT
              for(let x=line[ln][op-1].x;x<=line[ln][op].x;x++){
                map[line[ln][op].y][x]="\#"
              }
            }
            else {
              //LEFT
              for(let x=line[ln][op].x;x<=line[ln][op-1].x;x++){
                map[line[ln][op].y][x]="\#"
              }
            }
          }
          else {
            if(line[ln][op].y>line[ln][op-1].y){
              //DOWN
              for(let y=line[ln][op-1].y;y<=line[ln][op].y;y++){
                map[y][line[ln][op].x]="\#"
              }
            }
            else {
              //UP
              for(let y=line[ln][op].y;y<=line[ln][op-1].y;y++){
                map[y][line[ln][op].x]="\#"
              }
            }
          }
        }
      }

      //Simulate falling sand
      var start = {x:500, y:0}
      var steps = 0
      const MAX_STEPS = 1000000
      for(let c=0;c<MAX_STEPS;c++){
          var sandPos = this.simulateSand(start, map)
          if(sandPos==null){steps=c;break}
          map[sandPos.y][sandPos.x]="o"
      }
      this.outputMessage = steps
      var trace = this.traceSand(start,map)
      for(let i=0;i<trace.length;i++){map[trace[i].y][trace[i].x]="~"}
      this.outputVisual = this.stringifyMap(map)
    },
    calculateOutput2() {
      var line = this.inputMessage.split(/\n/)
      var X_MAX=0
      var Y_MAX=0
      //Read Input and calculate map size
      for(let idx=0;idx<line.length;idx++){
        line[idx] = line[idx].split(" -> ")
        for(let c=0;c<line[idx].length;c++){
          var t = line[idx][c].split(",")
          line[idx][c] = {x:parseInt(t[0]), y:parseInt(t[1])}
          //find the highest X and Y coordinates
          if(parseInt(t[0])>X_MAX){X_MAX=parseInt(t[0])}
          if(parseInt(t[1])>Y_MAX){Y_MAX=parseInt(t[1])}
        }
      }
      //Build empty map
      Y_MAX+=2
      X_MAX*=2
      var map = new Array(Y_MAX)
      for(let y=0;y<Y_MAX;y++){
        map[y] = new Array(X_MAX)
        for(let x=0;x<X_MAX;x++){
          map[y][x]="."
        }
      }
      //Populate map
      for(let ln=0;ln<line.length;ln++){
        for(let op=1;op<line[ln].length;op++){
          if(line[ln][op].x!=line[ln][op-1].x){
            if(line[ln][op].x>line[ln][op-1].x){
              //RIGHT
              for(let x=line[ln][op-1].x;x<=line[ln][op].x;x++){
                map[line[ln][op].y][x]="\#"
              }
            }
            else {
              //LEFT
              for(let x=line[ln][op].x;x<=line[ln][op-1].x;x++){
                map[line[ln][op].y][x]="\#"
              }
            }
          }
          else {
            if(line[ln][op].y>line[ln][op-1].y){
              //DOWN
              for(let y=line[ln][op-1].y;y<=line[ln][op].y;y++){
                map[y][line[ln][op].x]="\#"
              }
            }
            else {
              //UP
              for(let y=line[ln][op].y;y<=line[ln][op-1].y;y++){
                map[y][line[ln][op].x]="\#"
              }
            }
          }
        }
      }
      
      //Simulate falling sand
      var start = {x:500, y:0}
      var steps = 0
      const MAX_STEPS = 1000000
      for(let c=0;c<MAX_STEPS;c++){
          var sandPos = this.simulateSandTillFlooded(start, map)
          map[sandPos.y][sandPos.x]="o"
          if(sandPos.x==start.x&&sandPos.y==start.y){steps=c+1;break}
      }
      this.outputMessage2 = steps
      this.outputVisual = this.stringifyMap(map)
    },
    simulateSand(posIn, map) {
      var pos = {x:posIn.x, y:posIn.y}
      var isFalling = true
      while(isFalling){
        //DOWN ABYSS
        if(pos.y==map.length-1){return null}
        //DOWN
        else if(map[pos.y+1][pos.x]=="."){pos.y++}
        //DOWN-LEFT ABYSS
        else if(pos.x==0){return null}
        //DOWN-LEFT
        else if(map[pos.y+1][pos.x-1]=="."){pos.y++; pos.x--}
        //DOWN_RIGHT ABYSS
        else if(pos.x==map[pos.y].length-1){return null}
        //DOWN_RIGHT
        else if(map[pos.y+1][pos.x+1]=="."){pos.y++; pos.x++}
        //REST
        else {isFalling = false}
      }
      return(pos)
    },
    simulateSandTillFlooded(posIn, map) {
      var pos = {x:posIn.x, y:posIn.y}
      var isFalling = true
      while(isFalling){
        //DOWN ABYSS
        if(pos.y==map.length-1){return pos}
        //DOWN
        else if(map[pos.y+1][pos.x]=="."){pos.y++}
        //DOWN-LEFT ABYSS
        //else if(pos.x==0){return null}
        //DOWN-LEFT
        else if(map[pos.y+1][pos.x-1]=="."){pos.y++; pos.x--}
        //DOWN_RIGHT ABYSS
        //else if(pos.x==map[pos.y].length-1){return null}
        //DOWN_RIGHT
        else if(map[pos.y+1][pos.x+1]=="."){pos.y++; pos.x++}
        //REST
        else {isFalling = false}
      }
      return(pos)
    },
    traceSand(posIn,map){
      var pos = {x:posIn.x, y:posIn.y}
      var isFalling = true
      var out = []
      while(isFalling){
        out.push({x:pos.x, y:pos.y})
        //DOWN ABYSS
        if(pos.y==map.length-1){return out}
        //DOWN
        else if(map[pos.y+1][pos.x]=="."){pos.y++}
        //DOWN-LEFT ABYSS
        else if(pos.x==0){return out}
        //DOWN-LEFT
        else if(map[pos.y+1][pos.x-1]=="."){pos.y++; pos.x--}
        //DOWN_RIGHT ABYSS
        else if(pos.x==map[pos.y].length-1){return out}
        //DOWN_RIGHT
        else if(map[pos.y+1][pos.x+1]=="."){pos.y++; pos.x++}
        //REST
        else {return out}
      }
    },
    stringifyMap(map) {
      var out = ""
      const START_INDEX_X = 300
      const END_INDEX_X = 700
      for(let y=0;y<map.length;y++){
        for(let x=START_INDEX_X;x<END_INDEX_X;x++){
          out+=map[y][x]
        }
        out+="\n"
      }
      return out
    }
  }
}
</script>