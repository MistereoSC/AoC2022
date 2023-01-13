<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/12" target="_blank">Day 12: Hill Climbing Algorithm</a>      
    </div>
    <div>
      <textarea v-model="inputMessage" rows="12" cols="50" placeholder="Input"></textarea>
    </div>
    <div>
      <button @click="calculateOutput">Calculate</button>
      <input v-model="outputMessage" placeholder="Output Q1"/>
    </div>
    <div>
      <button @click="calculateOutput">Calculate</button>
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
      inputMessage: "",
      vis: [],
      map: []
    }
  },
  methods: {
    calculateOutput() {
      var posS = {x:0,y:0}
      var posE = {x:0,y:0}
      var map = this.inputMessage.split(/\n/)
      for(let y=0;y<map.length;y++){
        map[y]=map[y].split('')
        for(let x=0;x<map[y].length;x++){
          if(map[y][x]=="S"){
            map[y][x] = 1
            posS={x:x, y:y}
          }
          else if(map[y][x]=="E"){
            map[y][x] = 26
            posE={x:x, y:y}
          }
          else{map[y][x] = map[y][x].charCodeAt(0)-96}
        }
      }
      this.vis = new Array(map.length)
      for(let i=0;i<this.vis.length;i++){
        this.vis[i]=new Array(map[i].length)
        for(let j=0;j<this.vis[i].length;j++){this.vis[i][j]=9999}
      }
      this.map = map

      this.explore(posE,posS,0)
      this.outputMessage=this.vis[posS.y][posS.x]
      var min = 500
      for(let i=0;i<this.vis.length;i++){if(this.vis[i][0]<min){min=this.vis[i][0]}}
      this.outputMessage2 = min
    },
      explore(pos,end,acc){
        const MAX_ACC = 500
        if(acc>=this.vis[pos.y][pos.x] || acc>MAX_ACC){return}
        this.vis[pos.y][pos.x]=acc
        if(pos.x==end.x&&pos.y==end.y){return}
        acc++
        //W
        if(pos.x>0 && this.map[pos.y][pos.x-1]+1 >= this.map[pos.y][pos.x]){
          this.explore({x:pos.x-1, y:pos.y},end,acc)
        }
        //N
        if(pos.y>0 && this.map[pos.y-1][pos.x]+1 >= this.map[pos.y][pos.x]){
          this.explore({x:pos.x, y:pos.y-1},end,acc)
        }
        //S
        if(pos.y<this.map.length-1 && this.map[pos.y+1][pos.x]+1 >= this.map[pos.y][pos.x]){
          this.explore({x:pos.x, y:pos.y+1},end,acc)
        }
        //E
        if(pos.x<this.map[pos.y].length-1 && this.map[pos.y][pos.x+1]+1 >= this.map[pos.y][pos.x]){
          this.explore({x:pos.x+1, y:pos.y},end,acc)
        }
      }
    }
}
</script>