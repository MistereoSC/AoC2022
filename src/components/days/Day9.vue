<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/9" target="_blank">Day 9: Rope Bridge</a>      
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
      <textarea v-model="outputVisual" rows="300" cols="300" placeholder="Input"></textarea>
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
      outputVisual: "",
      inputMessage: ""
    }
  },
  methods: {
    calculateOutput() {
      var moves = this.inputMessage.split(/\n/)
      for(let i=0;i<moves.length;i++){
        moves[i] = {
          dir: moves[i].split(' ')[0],
          amt: parseInt(moves[i].split(' ')[1])
        }
      }
      var head = {x:200, y:200, prev:{x:200, y:200}}
      var tail = {x:200, y:200}
      var tailPositionStack = []
      tailPositionStack.push(new Object({x:200, y:200}))
      //R:78 L:146 U:159 D:82
      for(let c=0;c<moves.length;c++){
        for(let i=0;i<moves[c].amt;i++){
          head.prev.x = head.x
          head.prev.y = head.y
          switch(moves[c].dir){
            case "R":
              head.x++
              break
            case "L":
              head.x--
              break
            case "U":
              head.y--
              break
            case "D":
              head.y++
              break
          }
          if(Math.abs(head.x-tail.x)>1||Math.abs(head.y-tail.y)>1){ 
            tail.x = head.prev.x
            tail.y = head.prev.y
            tailPositionStack.push(new Object({x: tail.x, y:tail.y}))
          }
        }
      }
      tailPositionStack = tailPositionStack.filter((value, index, self) =>
        index === self.findIndex((t) => (
          t.x === value.x && t.y === value.y
      )))
      this.outputMessage = tailPositionStack.length
      this.outputVisual = this.textify([{x:head.x,y:head.y},{x:tail.x,y:tail.y}], tailPositionStack)
    },
    calculateOutput2() {
      var moves = this.inputMessage.split(/\n/)
      for(let i=0;i<moves.length;i++){
        moves[i] = {
          dir: moves[i].split(' ')[0],
          amt: parseInt(moves[i].split(' ')[1])
        }
      }
      var positions = []
      for(let k=0;k<=9;k++){positions.push(new Object({x:200, y:200}))}
      var tailPositionStack = []
      tailPositionStack.push(new Object({x:200, y:200}))

      for(let c=0;c<moves.length;c++){
        for(let i=0;i<moves[c].amt;i++){
          switch(moves[c].dir){
            case "R":
              positions[0].x++
              break
            case "L":
              positions[0].x--
              break
            case "U":
              positions[0].y--
              break
            case "D":
              positions[0].y++
              break
          }
          for(let k=1;k<positions.length;k++){
            var d = {x:0, y:0}
            d.x=positions[k-1].x - positions[k].x
            d.y=positions[k-1].y - positions[k].y
            if(Math.abs(positions[k-1].x - positions[k].x) <= 1 && Math.abs(d.y=positions[k-1].y - positions[k].y) <= 1) {break}
            //UP
            if(d.y==-2&&d.x==0){positions[k].y--}
            //UP-LEFT
            else if(d.y==-2&&d.x==-1){positions[k].y--;positions[k].x--}
            //UP-RIGHT
            else if(d.y==-2&&d.x==1){positions[k].y--;positions[k].x++}
            //DOWN
            else if(d.y==2&&d.x==0){positions[k].y++}
            //DOWN-LEFT
            else if(d.y==2&&d.x==-1){positions[k].y++;positions[k].x--}
            //DOWN-RIGHT
            else if(d.y==2&&d.x==1){positions[k].y++;positions[k].x++}
            //LEFT
            else if(d.y==0&&d.x==-2){positions[k].x--}
            //LEFT-UP
            else if(d.y==-1&&d.x==-2){positions[k].x--;positions[k].y--}
            //LEFT-DOWN
            else if(d.y==1&&d.x==-2){positions[k].x--;positions[k].y++}
            //RIGHT
            else if(d.y==0&&d.x==2){positions[k].x++}
            //RIGHT-UP
            else if(d.y==-1&&d.x==2){positions[k].x++;positions[k].y--}
            //RIGHT-DOWN
            else if(d.y==1&&d.x==2){positions[k].x++;positions[k].y++}
            //DIAG-UL
            else if(d.y==-2&&d.x==-2){positions[k].y--;positions[k].x--}
            //DIAG-UR
            else if(d.y==-2&&d.x==2){positions[k].y--;positions[k].x++}
            //DIAG-DL
            else if(d.y==2&&d.x==-2){positions[k].y++;positions[k].x--}
            //DIAG-DR
            else if(d.y==2&&d.x==2){positions[k].y++;positions[k].x++}
            
          }
          tailPositionStack.push(new Object({x: positions[positions.length-1].x, y:positions[positions.length-1].y}))
        }
      }

      tailPositionStack = tailPositionStack.filter((value, index, self) =>
        index === self.findIndex((t) => (
          t.x === value.x && t.y === value.y
      )))
      this.outputMessage2 = tailPositionStack.length
      this.outputVisual = this.textify(positions, tailPositionStack)
    },
    textify(positions, tailPositionStack){
      const maxY = 300
      const myxX = 300
      var arr=new Array(maxY)
      for(let y=0;y<maxY;y++){
        arr[y]=new Array(myxX)
        for(let x=0;x<myxX;x++){
          arr[y][x]='.'
        }
      }
      for(let k=0;k<tailPositionStack.length;k++){
        arr[tailPositionStack[k].y][tailPositionStack[k].x]="#"
      }
      for(let k=0;k<positions.length;k++){
        arr[positions[k].y][positions[k].x]=k
      }
      arr[200][200]="S"
      var out=""
      for(let y=0;y<maxY;y++){
        for(let x=0;x<myxX;x++){
          out+=arr[y][x]
        }
        out+="\n"
      }
      return out
    }
  }
}

</script>

