<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/13" target="_blank">Day  13: Distress Signal</a>      
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
      var set = this.inputMessage.split(/\n\s*\n/)
      var out = 0
      for(let idx=0;idx<set.length;idx++){
        var split = set[idx].split(/\n/)
        if( this.isInRightOrder(JSON.parse(split[0]), JSON.parse(split[1])) ) { out += idx+1 }
      }
      this.outputMessage = out
    },
    calculateOutput2() {
      var set = this.inputMessage.replace(/\n\s*\n/gm,'\n').split(/\n/)
      set.push("[[2]]")
      set.push("[[6]]")
      var set2 = []
      for(let idx=0;idx<set.length;idx++){
        set2.push(JSON.parse(set[idx]))
      }
      console.log(set2)
      var out = this.quickSort(set2)
      console.log(out)
      var idx1=0
      var idx2=0
      for(let idx=0;idx<out.length;idx++){
        if(JSON.stringify(out[idx])=="[[2]]"){idx1=idx+1}
        else if(JSON.stringify(out[idx])=="[[6]]"){idx2=idx+1}
      }
      console.log(idx1+"*"+idx2)
      this.outputMessage2=idx1*idx2
    },
    isInRightOrder(left, right) {
      if(left.length==0&&right.length==0){return null}
      for(let idx=0;idx<left.length;idx++){
        if(idx>=right.length){
          return false
        }
        if(!Array.isArray(left[idx])&&!Array.isArray(right[idx])){
          if(left[idx]>right[idx]){return false}
          if(left[idx]<right[idx]){return true}
        }
        else if(Array.isArray(left[idx])&&Array.isArray(right[idx])){
          switch (this.isInRightOrder(left[idx],right[idx])){
            case false:
              return false
            case true:
              return true
          }
        }
        else if(Array.isArray(left[idx])&&!Array.isArray(right[idx])){
          switch (this.isInRightOrder(left[idx],[right[idx]])){
            case false:
              return false
            case true:
              return true
          }
        }
        else if(!Array.isArray(left[idx])&&Array.isArray(right[idx])){
          switch (this.isInRightOrder([left[idx]],right[idx])){
            case false:
              return false
            case true:
              return true
          }
        }
      }
      return true
    },
    swap(arr, a, b) {
      let tmp=arr[a]
      arr[a]=arr[b]
      arr[b]=tmp
    },
    quickSort(arr){
      if(arr.length<=1){return arr}
      var pivot = arr[0]
      var left = []
      var right = []
      for(let idx=1;idx<arr.length;idx++){
        if(this.isInRightOrder(arr[idx],pivot)){left.push(arr[idx])}
        else{right.push(arr[idx])}
      }
      var out = []
      out.push(...this.quickSort(left))
      out.push(pivot)
      out.push(...this.quickSort(right))
      return out
    }
  }
}
</script>