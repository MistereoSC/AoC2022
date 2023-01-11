<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/7" target="_blank">Day 7: No Space Left On Device</a>      
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
      filesystem: {},
      dirStack: []
    }
  },
  methods: {
    calculateOutput() {
      this.outputMessage=0
      this.filesystem={root:{type:"dir",name:"root",size:0}}
      this.dirStack=[]
      var program = this.inputMessage.split(/\n/)
      var pointer = this.filesystem.root
      var pointerStack = []
      var programCount = 1

      for(programCount;programCount<program.length;programCount++){
        var args = program[programCount].split(" ")
        if(args[0]=="$"){
          if(args[1]=="ls"){continue}
          if(args[1]=="cd"){
            if(args[2]==".."){
              pointerStack.pop()
              pointer=this.filesystem.root
              for(let i=0;i<pointerStack.length;i++){
                pointer=pointer[pointerStack[i]]
              }
            }
            else{
              pointerStack.push(args[2])
              pointer=pointer[args[2]]
            }
          }
        }
        else if(args[0]=="dir"){
          pointer[args[1]]={type:"dir",name:args[1],size:0}
        }
        else {
          pointer[args[1]]={type:"file",name:args[1],size:parseInt(args[0])}
        }
      }
      
      this.calculateSize(this.filesystem.root)
      this.outputMessage=0
      this.sumDirectorySize(this.filesystem.root)
      this.findDirectoryToDelete(this.filesystem.root)
      console.log(this.dirStack)
      this.outputMessage2=Math.min(...this.dirStack)
    },
    calculateSize(ptr){
      var acc=0
      if(ptr.type=="dir"){
        for(var key in ptr){
          if(key!=="type"&&key!=="size"&&key!=="name"){            
            acc+=this.calculateSize(ptr[key])
          }
        }
        ptr.size=acc
        return acc
      }
      else{
        return ptr.size
      }
    },
    sumDirectorySize(ptr){
      const limit = 100000
      if(ptr.type=="dir"){
        if(ptr.size<=limit){this.outputMessage+=ptr.size}
        for(var key in ptr){
          if(key!=="type"&&key!=="size"&&key!=="name"){            
            this.sumDirectorySize(ptr[key])
          }
        }
      }
    },
    findDirectoryToDelete(ptr){
      const lmt = this.filesystem.root.size-40000000
      if(ptr.type=="dir"){
        if(ptr.size>=lmt){
          this.dirStack.push(ptr.size)
        }
        for(var key in ptr){
          if(key!=="type"&&key!=="size"&&key!=="name"){            
            this.findDirectoryToDelete(ptr[key])
          }
        }
      }
    }
  }
}
</script>