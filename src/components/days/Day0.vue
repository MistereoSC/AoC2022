<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/1" target="_blank">Day 0: Template</a>      
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

    }
  }
}
</script>