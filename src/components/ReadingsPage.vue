<template>
  <div class="w-8/12 mx-auto">
    <div class="flex flex-col">
      <div class="overflow-x-auto sm:rounded-lg">
        <div class="inline-block min-w-full align-middle">
          <div class="overflow-hidden">
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
                    Mac Address
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
                  v-for="reading in readingArray"
                  v-bind:key="reading.id">
                  <td
                    class="py-1 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ reading.id }}
                  </td>
                  <td
                    class="py-1 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ reading.macAddressSensor }}
                  </td>
                  <td
                    class="py-1 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ reading.openedBy }}
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
export default defineComponent({
  data() {
    return {
      readingArray: this.readings,
    }
  },
  methods: {
    async getReadings() {
      let url = "https://localhost:7220/api/Readings"
      let response = await axios.get(url)
      this.readingArray = response.data
      this.$emit("updateReadings", this.readingArray)
    },
  },
  mounted() {
    if(!this.readingArray.length){
      this.getReadings()
      console.log("Getting readings")
    }
  },
  props:{
    readings:{
      required: true,
      type: Object as PropType<readingType[]>
    }
  }
})
</script>
