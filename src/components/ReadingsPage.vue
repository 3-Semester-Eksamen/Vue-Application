<template>
  <div class="w-8/12 mx-auto">
    <div class="flex flex-col">
      <div class="overflow-x-auto sm:rounded-lg">
        <div class="inline-block min-w-full align-middle">
          <div class="overflow-hidden">
            <div class="my-4 grid grid-cols-12">
              <div class="col-span-3 inline-flex">
                <input
                  type="text"
                  placeholder="Lock Name"
                  v-model="filterByLockString"
                  class="pl-4" />
                <button
                  class="bg-green-700 text-gray-300 hover:bg-gray-700 hover:text-white px-3 rounded-md text-sm font-medium py-2"
                  @click="filterByLock">
                  Filter
                </button>
              </div>
              <div class="col-span-3 inline-flex">
                <input
                  type="text"
                  placeholder="User Name"
                  v-model="filterByUserString"
                  class="pl-4" />
                <button
                  class="bg-green-700 text-gray-300 hover:bg-gray-700 hover:text-white px-3 rounded-md text-sm font-medium py-2"
                  @click="filterByUser">
                  Filter
                </button>
              </div>
            </div>
            <table class="min-w-full divide-y divide-gray-700 table-fixed">
              <thead class="bg-gray-700">
                <tr>
                  <th
                    scope="col"
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white">
                    Id
                  </th>
                  <th
                    scope="col"
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white">
                    Lock Name
                  </th>
                  <th
                    scope="col"
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white">
                    User
                  </th>
                  <th
                    scope="col"
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white">
                    Timestamp
                  </th>
                </tr>
              </thead>
              <tbody class="bg-gray-800 divide-y divide-gray-700">
                <tr
                  class="hover:bg-gray-700"
                  v-for="reading in filteredReadingArray"
                  v-bind:key="reading.id">
                  <td
                    class="py-1 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ reading.id }}
                  </td>
                  <td
                    class="py-1 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ sensorNameByMac(reading.macAddressSensor) }}
                  </td>
                  <td
                    class="py-1 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ userNameById(reading.openedBy) }}
                  </td>
                  <td
                    class="py-1 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ reading.time }}
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
import { defineComponent } from "vue"
import type { PropType } from "vue"
import type { readingType } from "../types/readingType"
import axios from "axios"
import type { sensorType } from "@/types/sensorType"
import type { keyType } from "../types/keyType"
export default defineComponent({
  data() {
    return {
      readingArray: this.readings,
      sensorArray: this.sensors,
      unassignedArray: [],
      filteredReadingArray: [] as readingType[],
      keyArray: this.keys,
      filterByLockString: "",
      filterByUserString: "",
      filterByTimeString: ""
    }
  },
  methods: {
    async getReadings() {
      let url = "https://localhost:7220/api/Readings"
      let response = await axios.get(url)
      this.readingArray = response.data
      this.$emit("updateReadings", this.readingArray)
    },
    async getSensors() {
      let assignedUrl = "https://localhost:7220/api/Sensors"
      let assignedResponse = await axios.get(assignedUrl)
      this.sensorArray = assignedResponse.data
      let unassignedUrl = "https://localhost:7220/api/Sensors?unassigned=true"
      let unassginedResponse = await axios.get(unassignedUrl)
      this.unassignedArray = unassginedResponse.data
      this.$emit("updateSensors", this.sensorArray, this.unassignedArray)
    },
    async getKeys() {
      let url = "https://localhost:7220/api/Keys"
      let response = await axios.get(url)
      this.keyArray = response.data
      this.$emit("updateKeys", this.keyArray)
    },
    sensorNameByMac(mac: string) {
      let sensor = this.sensorArray.find((sens) => sens.macAddress === mac)
      if (!sensor) return mac
      return sensor.name
    },
    userNameById(id: number) {
      let user = this.keyArray.find((user) => user.id === id)
      if (!user) return id
      return user.name
    },
    async getAll(){
      await this.getReadings()
      await this.getSensors()
      await this.getKeys()
    },
    async filterByLock(){
      if(!this.filterByLockString){
        this.filteredReadingArray = this.readingArray
        return;
      }
      const filteredLocks = this.sensorArray.filter(sensor => {
        return sensor.name.toLowerCase().includes(this.filterByLockString.toLowerCase())
      })

      const matchArray: readingType[] = []
      this.readingArray.forEach(reading => {
        filteredLocks.forEach(lock => {
          if(lock.macAddress === reading.macAddressSensor) matchArray.push(reading)
        })
      })
      this.filteredReadingArray = matchArray
    },
    async filterByUser(){
      if(!this.filterByUserString){
        this.filteredReadingArray = this.readingArray
        return;
      }
      const filteredUsers = this.keyArray.filter(key => {
        return key.name.toLowerCase().includes(this.filterByUserString.toLowerCase())
      })

      const matchArray: readingType[] = []
      this.readingArray.forEach(reading => {
        filteredUsers.forEach(user => {
          if(user.id === reading.openedBy) matchArray.push(reading)
        })
      })
      this.filteredReadingArray = matchArray
    }
  },
  async mounted() {
    if (!this.readingArray.length) {
      await this.getAll()
      console.log("Getting all data")
    }
    this.filteredReadingArray = this.readingArray
  },
  props: {
    readings: {
      required: true,
      type: Object as PropType<readingType[]>,
    },
    sensors: {
      required: true,
      type: Object as PropType<sensorType[]>,
    },
    keys: {
      required: true,
      type: Object as PropType<keyType[]>,
    },
  },
  computed: {},
})
</script>
