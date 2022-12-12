<template>
  <div class="w-8/12 mx-auto">
    <div class="flex flex-col" v-if="sensorArray.length">
      <div class="overflow-x-auto sm:rounded-lg">
        <div class="inline-block min-w-full align-middle">
          <div class="overflow-hidden">
            <div class="my-4 grid grid-cols-12">
              <div class="col-span-3 inline-flex">
                <input
                  type="text"
                  placeholder="Name or MacAddress"
                  v-model="filterValue"
                  class="pl-4" />
                <button
                  class="bg-green-700 text-gray-300 hover:bg-gray-700 hover:text-white px-3 rounded-md text-sm font-medium py-2"
                  @click="filterByInput(filterValue)">
                  Filter
                </button>
              </div>
              <div></div>
            </div>
            <table
              class="min-w-full divide-y divide-gray-700 table-fixed grid grid-cols-5">
              <thead class="bg-gray-700 col-span-5">
                <tr class="grid grid-cols-5">
                  <th
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white col-span-1">
                    Assigned Name
                  </th>
                  <th></th>
                  <th
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white col-span-1">
                    MAC-Address
                  </th>
                  <th></th>
                  <th></th>
                </tr>
              </thead>
              <tbody class="bg-gray-800 divide-y divide-gray-700 col-span-5">
                <tr
                  class="hover:bg-gray-700 grid grid-cols-5"
                  v-for="sensor in sensorArray"
                  v-bind:key="sensor.macAddress">
                  <sensorTableEditRow
                    :sensor="sensor"
                    class="w-full"
                    @updateSensor="editSensor"
                    @deleteSensor="deleteSensor" />
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
    <div class="flex flex-col mt-12" v-if="unassignedSensors.length">
      <div class="overflow-x-auto sm:rounded-lg">
        <div class="inline-block min-w-full align-middle">
          <div class="overflow-hidden">
            <table
              class="min-w-full divide-y divide-gray-700 table-fixed grid grid-cols-5">
              <thead class="bg-gray-700 col-span-5">
                <tr class="grid grid-cols-5">
                  <th
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white col-span-2">
                    Unassigned Name
                  </th>
                  <th
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white col-span-2">
                    MAC-Address
                  </th>
                  <th></th>
                </tr>
              </thead>
              <tbody class="bg-gray-800 divide-y divide-gray-700 col-span-5">
                <tr
                  class="hover:bg-gray-700 grid grid-cols-5"
                  v-for="sensor in unassignedArray"
                  v-bind:key="sensor.macAddress">
                  <sensorTableUnassignedRow
                    :sensor="sensor"
                    class="w-full"
                    @updateSensor="editSensor"
                    @deleteSensor="deleteSensor" />
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent } from "vue"
import type { sensorType } from "../types/sensorType"
import type { PropType } from "vue"
import axios from "axios"
import sensorTableEditRow from "./organism/sensorTableEditRow.vue"
import sensorTableUnassignedRow from "./organism/sensorTableUnassignedRow.vue"

export default defineComponent({
  data() {
    return {
      sensorArray: this.sensors,
      unassignedArray: this.unassignedSensors,
      filterValue: "",
      isEditing: false,
    }
  },
  components: {
    sensorTableEditRow,
    sensorTableUnassignedRow,
  },
  props: {
    sensors: {
      required: true,
      type: Object as PropType<sensorType[]>,
    },
    unassignedSensors: {
      required: true,
      type: Object as PropType<sensorType[]>,
    },
  },
  methods: {
    async getSensors() {
      let assignedUrl = "https://localhost:7220/api/Sensors"
      let assignedResponse = await axios.get(assignedUrl)
      this.sensorArray = assignedResponse.data
      let unassignedUrl = "https://localhost:7220/api/Sensors?unassigned=true"
      let unassginedResponse = await axios.get(unassignedUrl)
      this.unassignedArray = unassginedResponse.data
      this.$emit("updateSensors", this.sensorArray, this.unassignedArray)
    },
    async deleteSensor(sensor: sensorType) {
      const url = "https://localhost:7220/api/Sensors"
      sensor.name = "Unassigned"
      const response = await axios.put(url, sensor)
      await this.getSensors()
    },
    async editSensor(sensor: sensorType) {
      const url = "https://localhost:7220/api/Sensors"
      const response = await axios.put(url, sensor)
      let assignedResponse = await axios.get(url)
      this.sensorArray = assignedResponse.data
      await this.getSensors()
    },
    async filterByInput(input: string) {
      await this.getSensors()
      const tempArray: any = this.sensorArray.filter((s) => {
        if (s.name.toLowerCase().includes(input.toLowerCase())) return true
        else if (s.macAddress.toLowerCase().includes(input.toLowerCase()))
          return true
        else return false
      })
      if (tempArray) this.sensorArray = tempArray
    },
  },
  mounted() {
    if (!this.sensorArray.length) {
      this.getSensors()
      console.log("Getting Sensors")
    }
  },
})
</script>
