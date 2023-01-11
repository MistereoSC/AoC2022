<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/8" target="_blank">Day 8: Treetop Tree House</a>      
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
      var treeArray = this.inputMessage.split(/\n/)
      var visibleTrees = 0
      for(let i=0;i<treeArray.length;i++){treeArray[i]=treeArray[i].split('')}

      for(let y=0;y<treeArray.length;y++){
        for(let x=0;x<treeArray[y].length;x++){
          if(this.isEdge(x,y,treeArray)||this.isVisibleFromLeft(x,y,treeArray)||this.isVisibleFromRight(x,y,treeArray)||this.isVisibleFromTop(x,y,treeArray)||this.isVisibleFromBottom(x,y,treeArray)){
            visibleTrees++
          }
        }
      }
      this.outputMessage=visibleTrees
    },
    isVisibleFromLeft(x,y,arr){
      for(let i=0;i<x;i++){
        if(arr[y][i]>=arr[y][x]){return false}
      }
      return true
    },
    isVisibleFromRight(x,y,arr){
      for(let i=x+1;i<arr[y].length;i++){
        if(arr[y][i]>=arr[y][x]){return false}
      }
      return true
    },
    isVisibleFromTop(x,y,arr){
      for(let i=0;i<y;i++){
        if(arr[i][x]>=arr[y][x]){return false}
      }
      return true
    },
    isVisibleFromBottom(x,y,arr){
      for(let i=y+1;i<arr.length;i++){
        if(arr[i][x]>=arr[y][x]){return false}
      }
      return true
    },
    isEdge(x,y,arr){
      if(y==0||x==0||y==arr.length-1||x==arr[y].length-1){return true}
      else{return false}
    },
    calculateOutput2() {
      var treeArray = this.inputMessage.split(/\n/)
      for(let i=0;i<treeArray.length;i++){treeArray[i]=treeArray[i].split('')}
      var stack = []

      for(let y=0;y<treeArray.length;y++){
        for(let x=0;x<treeArray[y].length;x++){
          stack.push(this.distanceLeft(x,y,treeArray)*this.distanceRight(x,y,treeArray)*this.distanceTop(x,y,treeArray)*this.distanceBottom(x,y,treeArray))
        }
      }

      this.outputMessage2=Math.max(...stack)
    },
    distanceLeft(x,y,arr){
      var acc = 0
      for(let i=x-1;i>=0;i--){
        if(arr[y][i]<arr[y][x]){acc++}
        else{acc++; break}
      }
      return acc
    },
    distanceRight(x,y,arr){
      var acc = 0
      for(let i=x+1;i<arr[y].length;i++){
        if(arr[y][i]<arr[y][x]){acc++}
        else{acc++; break}
      }
      return acc
    },
    distanceTop(x,y,arr){
      var acc = 0
      for(let i=y-1;i>=0;i--){
        if(arr[i][x]<arr[y][x]){acc++}
        else{acc++; break}
      }
      return acc
    },
    distanceBottom(x,y,arr){
      var acc = 0
      for(let i=y+1;i<arr.length;i++){
        if(arr[i][x]<arr[y][x]){acc++}
        else{acc++; break}
      }
      return acc
    }
  }
}
</script>