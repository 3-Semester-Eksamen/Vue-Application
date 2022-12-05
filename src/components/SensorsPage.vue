<template>
    <div class="w-8/12 mx-auto">
    <div class="flex flex-col">
      <div class="overflow-x-auto shadow-md sm:rounded-lg">
        <div class="inline-block min-w-full align-middle">
          <div class="overflow-hidden">
            <table class="min-w-full divide-y divide-gray-700 table-fixed">
              <thead class="bg-gray-700">
                <tr>
                  <th
                    scope="col"
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white">
                    Name
                  </th>
                  <th
                    scope="col"
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white">
                    MAC-Address
                  </th>
                </tr>
              </thead>
              <tbody class="bg-gray-800 divide-y divide-gray-700">
                <tr
                  class="hover:bg-gray-700"
                  v-for="sensor in sensorArray"
                  v-bind:key="sensor.macAddress">
                  <td
                    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ sensor.name }}
                  </td>
                  <td
                    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ sensor.macAddress }}
                  </td>
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
import { defineComponent } from 'vue'
import type { sensorType } from "../types/sensorType"
import type { PropType } from 'vue'
import axios from 'axios'

export default defineComponent({
    data() {
        return {
            sensorArray: this.sensors
        }
    },
    props: {
        sensors: {
            required: true,
            type: Object as PropType<sensorType[]>
        }
    },
    methods: {
        async getSensors() {
            let url = "https://localhost:7220/api/Sensors"
            let response = await axios.get(url)
            this.sensorArray = response.data
            this.$emit("updateSensors", this.sensorArray)
        }
    },
    mounted() {
        if(!this.sensorArray.length) {
            this.getSensors()
            console.log("Getting Sensors")
        }
    }
})
</script>
