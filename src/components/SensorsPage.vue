<template>
  <div class="w-8/12 mx-auto">
    <div class="flex flex-col">
      <div class="overflow-x-auto sm:rounded-lg">
        <div class="inline-block min-w-full align-middle">
          <div class="overflow-hidden">
            <table
              class="min-w-full divide-y divide-gray-700 table-fixed grid grid-cols-5">
              <thead class="bg-gray-700 col-span-5">
                <tr class="grid grid-cols-5">
                  <th
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white col-span-1">
                    Unassigned Name
                  </th>
                  <th></th>
                  <th></th>
                  <th
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white col-span-1">
                    MAC-Address
                  </th>
                  <th></th>
                </tr>
              </thead>
              <tbody class="bg-gray-800 divide-y divide-gray-700 col-span-5">
                <tr
                  class="hover:bg-gray-700 grid grid-cols-5"
                  v-for="sensor in sensorArray"
                  v-bind:key="sensor.macAddress">
                  <td
                    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap col-span-1">
                    {{ sensor.name }}
                  </td>
                  <td></td>
                  <td></td>
                  <td
                    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap col-span-1">
                    {{ sensor.macAddress }}
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
    <div class="flex flex-col mt-12">
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
                  <td
                    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap col-span-2">
                    {{ sensor.name }}
                  </td>
                  <td
                    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap col-span-2">
                    {{ sensor.macAddress }}
                  </td>
                  <td></td>
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

export default defineComponent({
  data() {
    return {
      sensorArray: this.sensors,
      unassignedArray: this.unassignedSensors,
    }
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
  },
  mounted() {
    if (!this.sensorArray.length) {
      this.getSensors()
      console.log("Getting Sensors")
    }
  },
})
</script>
