<template>
  <div class="app">
    <div class="bg-gray">
      <HeaderBar @toggle="switchView" />
      <UserKeysPage v-if="state == 1" :keys="keyArray" @updateKeys="updateKeys"/>
      <ReadingsPage v-if="state == 2" :readings="readingArray" @updateReadings="updateReadings"/>
      <SensorsPage v-if="state == 3" :sensors="sensorArray" @updateSensors="updateSensors"/>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import UserKeysPage from "./components/UserKeysPage.vue";
import ReadingsPage from "./components/ReadingsPage.vue"
import HeaderBar from "./components/HeaderBar.vue";
import SensorsPage from "./components/SensorsPage.vue"
import type { readingType } from "./types/readingType"
import type { keyType } from "./types/keyType"
import type { sensorType } from "./types/sensorType" 
let readings: readingType[] = []
let keys: keyType[] = []
let sensors: sensorType[] = []
export default defineComponent({
  name: "App",
  // type inference enabled
  components: {
    HeaderBar,
    UserKeysPage,
    ReadingsPage,
    SensorsPage,
  },
  data() {
    return {
      state: 1,
      readingArray: readings,
      keyArray: keys,
      sensorArray: sensors
    };
  },
  methods: {
    switchView(num: number) {
      this.state = num;
    },
    updateKeys(keys: keyType[]){
      this.keyArray = keys
    },
    updateReadings(readings: readingType[]){
      this.readingArray = readings
    },
    updateSensors(sensors: sensorType[]){
      this.sensorArray = sensors
    }
  },
});
</script>
