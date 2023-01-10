<template>
  <div class="main">
    <div class="select-frame">
      <DaySelect @selectIndex="(idx)=>selectedIndex=idx"></DaySelect>
    </div>
    <div class="day-frame">
      <h2>Advent of Code 2022</h2>
      <Day1 v-if="selectedIndex==0"></Day1>
      <Day2 v-if="selectedIndex==1"></Day2>
      <Day3 v-if="selectedIndex==2"></Day3>
      <Day4 v-if="selectedIndex==3"></Day4>
      <Day5 v-if="selectedIndex==4"></Day5>
    </div>
  </div>
</template>

<style scoped>
.select-frame{
  position: fixed;
  top: 0;
}
.day-frame{
  margin-left: 236px;
  padding-left: 12px;
  border-left: 2px solid var(--color-accent-1);
  min-height: 95vh;
}
</style>

<script>
import DaySelect from "@/components/DaySelect.vue"
import Day1 from "@/components/days/Day1.vue"
import Day2 from "@/components/days/Day2.vue"
import Day3 from "@/components/days/Day3.vue"
import Day4 from "@/components/days/Day4.vue"
import Day5 from "@/components/days/Day5.vue"

export default {
  components: {DaySelect, Day1, Day2, Day3, Day4, Day5},
  data() {
    return {
      selectedIndex: 0
    }
  }
}
</script>