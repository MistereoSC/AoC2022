<template>
  <div class="main">
    <ul>
      <p class="header-text">Select Day</p>
      <li class="item" @click="$emit('selectIndex',0)" >Day  1:  Calorie Counting</li>
      <li class="item" @click="$emit('selectIndex',1)" >Day  2:  Rock Paper Scissors</li>
      <li class="item" @click="$emit('selectIndex',2)" >Day  3:  Rucksack Reorganization</li>
      <li class="item" @click="$emit('selectIndex',3)" >Day  4:  Camp Cleanup</li>
      <li class="item" @click="$emit('selectIndex',4)" >Day  5:  Supply Stacks</li>
      <li class="item" @click="$emit('selectIndex',5)" >Day  6:  Tuning Trouble</li>
      <li class="item" @click="$emit('selectIndex',6)" >Day  7:  No Space Left On Device</li>
      <li class="item" @click="$emit('selectIndex',7)" >Day  8:  Treetop Tree House</li>
      <li class="item" @click="$emit('selectIndex',8)" >Day  9:  Rope Bridge</li>
      <li class="item" @click="$emit('selectIndex',9)" >Day  10: Cathode-Ray Tube</li>
      <li class="item" @click="$emit('selectIndex',10)">Day  11: Monkey in the Middle</li>
      <li class="item" @click="$emit('selectIndex',11)">Day  12: Hill Climbing Algorithm</li>
      <li class="item disabled">Day  13: Distress Signal</li>
      <li class="item disabled">Day  14: Regolith Reservoir</li>
      <li class="item disabled">Day  15: Beacon Exclusion Zone</li>
      <li class="item disabled">Day  16: Proboscidea Volcanium</li>
      <li class="item disabled">Day  17: Pyroclastic Flow</li>
      <li class="item disabled">Day  18: Boiling Boulders</li>
      <li class="item disabled">Day  19: Not Enough Minerals</li>
      <li class="item disabled">Day  20: Grove Positioning System</li>
      <li class="item disabled">Day  21: Monkey Math</li>
      <li class="item disabled">Day  22: Monkey Map</li>
      <li class="item disabled">Day  23: Unstable Diffusion</li>
      <li class="item disabled">Day  24: Blizzard Basin</li>
      <li class="item disabled">Day  25: Full of Hot Air</li>
    </ul>
  </div>
</template>

<style scoped>
.main{
  display: flex;
}
.item{
  color: var(--color--text);
}
.item:hover{
  color: var(--color-text-highlight);
}
.item.disabled{
  color: var(--color-text-disabled);
}
p{
  margin: 2px 8px;
}
ul{
  list-style-type: none;
  padding: 0px 12px 0px 4px;
  margin: 0px 4px;
  cursor: default;
}
</style>

<script>
export default {
  emits: ['selectIndex']
}
</script>