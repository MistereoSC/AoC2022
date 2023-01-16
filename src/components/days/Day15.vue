<template>
  <div class="main">
    <div>
      <a href="https://adventofcode.com/2022/day/15" target="_blank">Day 15: Beacon Exclusion Zone</a>      
    </div>
    <div>
      <textarea v-model="inputMessage" rows="12" cols="50" placeholder="Input"></textarea>
    </div>
    <div>
      <button @click="calculateOutput(0)">Calculate</button>
      <input v-model="outputMessage" placeholder="Output"/>
    </div>
    <div>
      <button @click="calculateOutput(1)">Calculate</button>
      <input v-model="outputMessage2" placeholder="Output"/>
    </div>
    <div>
      <textarea v-model="outputVisuals" rows="12" cols="50" placeholder="Output Visuals"></textarea>
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
      outputVisuals: ""
    }
  },
  methods: {
    calculateOutput(prt) {
      var Y_MIN = 100000000
      var X_MIN = 100000000
      var X_MAX = -100000000
      var Y_MAX = -100000000
      var Y_TO_CHECK = 2000000
      
      var lines = this.inputMessage.split(/\n/)
      var sensors = []
      var beacons = []
      //Read inputs
      for(let idx=0;idx<lines.length;idx++){
        var ln = lines[idx].replace(',','').split(' ')
        sensors.push({x:parseInt(ln[2].substring(2)), y:parseInt(ln[3].substring(2)), distance:0})
        beacons.push({x:parseInt(ln[8].substring(2)), y:parseInt(ln[9].substring(2))})
        //Calculate distance to closest beacon for each sensor
        sensors[idx].distance = Math.abs(sensors[idx].x - beacons[idx].x) + Math.abs(sensors[idx].y - beacons[idx].y)
        //Calculate area size
        if(sensors[idx].x-sensors[idx].distance<X_MIN){X_MIN=sensors[idx].x-sensors[idx].distance}
        if(sensors[idx].x+sensors[idx].distance>X_MAX){X_MAX=sensors[idx].x+sensors[idx].distance}
        if(sensors[idx].y-sensors[idx].distance<Y_MIN){Y_MIN=sensors[idx].y-sensors[idx].distance}
        if(sensors[idx].y+sensors[idx].distance>Y_MAX){Y_MAX=sensors[idx].y+sensors[idx].distance}
      }
      //Filter duplicate beacons and check if they are in range
      var filteredBeacons = beacons.filter((value, index, self) =>
      index === self.findIndex((t) => (
        t.x === value.x && t.y === value.y
        )))
      

      if(prt==0){
        //Filter sensors and beacons in range
        var sensorsInRange = []
        for(let idx=0;idx<sensors.length;idx++){
          if(sensors[idx].distance>=Math.abs(sensors[idx].y - Y_TO_CHECK)){sensorsInRange.push(sensors[idx])}
        }
        var beaconsInRange = 0
        for(let idx=0;idx<filteredBeacons.length;idx++){
          if(filteredBeacons[idx].y==Y_TO_CHECK){beaconsInRange+=1}
        }
        //Check each field on y=Y_TO_CHECK if it is in distance of any sensor
        var acc=0
        for(let x=X_MIN;x<=X_MAX;x++){
          for(let idx=0;idx<sensorsInRange.length;idx++){
            if(this.isInRange({y:Y_TO_CHECK, x:x}, sensorsInRange[idx])){acc++;break}
          }
        }
        acc-=beaconsInRange
        this.outputMessage=acc
      }
      if(prt==1){
        //0 to 20 / 4000000
        const C_FROM = 0
        const C_TO = 4000000
        //Find all fields that may contain the lost beacon
        var possibleFields = []
        console.log("Checking "+sensors.length+" sensors")
        for(let idx=0;idx<sensors.length;idx++){
          console.log("Sensor: "+idx+" ("+sensors[idx].distance+")")
          var pt = []
          for(let c=0;c<=sensors[idx].distance+1;c++){
            var v = sensors[idx].distance+1-c
            pt.push({y:sensors[idx].y+c, x:sensors[idx].x+v})
            pt.push({y:sensors[idx].y+c, x:sensors[idx].x-v})
            pt.push({y:sensors[idx].y-c, x:sensors[idx].x+v})
            pt.push({y:sensors[idx].y-c, x:sensors[idx].x-v})
          }
          for(let c=0;c<pt.length;c++){
            if(pt[c].x>=C_FROM&&pt[c].x<=C_TO && pt[c].y>=C_FROM&&pt[c].y<=C_TO){
              var oor=true
              for(let s=0;s<sensors.length;s++){
                if(this.isInRange(pt[c],sensors[s])){oor=false;break}
              }
              if(oor){possibleFields.push(pt[c])}
            }
          }
        }
        console.log(possibleFields)
        this.outputMessage2 = possibleFields[0].x*4000000+possibleFields[0].y
      }
    },
    isInRange(point,sensor){
      if(Math.abs(point.x-sensor.x)+Math.abs(point.y-sensor.y)<=sensor.distance){return true}
      return false
    }
  }
}
</script>