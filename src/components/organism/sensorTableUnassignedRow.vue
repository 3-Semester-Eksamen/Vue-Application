<template>
  <td
    v-if="isEditing"
    class="py-4 px-6 text-sm font-medium text-black whitespace-nowrap col-span-1">
    <input type="text" v-model="editSensor.name" class="w-full" />
  </td>
  <td v-if="isEditing"></td>
  
  <td
    v-if="isEditing"
    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap col-span-1">
    {{ editSensor.macAddress }}
  </td>
  <td v-if="isEditing"></td>
  <td
    v-if="isEditing"
    class="py-2 px-6 text-sm font-medium text-white whitespace-nowrap col-span-1">
    <div class="flex">
      <div class="px-5">
        <button
          class="bg-green-700 text-gray-300 hover:bg-blue-900 hover:text-white px-3 rounded-md text-sm font-medium py-2 w-20"
          @click="updateSensor">
          Confirm
        </button>
      </div>
      <div>
        <button
          class="bg-red-700 text-gray-300 hover:bg-red-900 hover:text-white px-3 rounded-md text-sm font-medium py-2 w-20"
          @click="cancelUpdate">
          Cancel
        </button>
      </div>
    </div>
  </td>
  <td
    v-if="!isEditing"
    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap col-span-1">
    {{ sensor.name }}
  </td>
  <td v-if="!isEditing"></td>
  
  <td
    v-if="!isEditing"
    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap col-span-1">
    {{ sensor.macAddress }}
  </td>
  <td v-if="!isEditing"></td>
  <td
    v-if="!isEditing"
    class="py-2 px-6 text-sm font-medium text-white whitespace-nowrap col-span-1">
    <div class="flex">
      <div class="px-5">
        <button
          class="bg-blue-700 text-gray-300 hover:bg-blue-900 hover:text-white px-3 rounded-md text-sm font-medium py-2 w-20"
          @click="isEditing = !isEditing">
          Assign
        </button>
      </div>
    </div>
  </td>
</template>
<script lang="ts">
import type { sensorType } from "../../types/sensorType"
import { defineComponent } from "vue"
import type { PropType } from "vue"

export default defineComponent({
  data() {
    return {
      editSensor: this.sensor,
      isEditing: false,
    }
  },
  props: {
    sensor: {
      required: true,
      type: Object as PropType<sensorType>,
    },
  },
  emits:{
    updateSensor: String,
    deleteSensor: String
  },
  methods: {
    updateSensor() {
      this.$emit("updateSensor", this.editSensor)
      this.isEditing = false
    },
    cancelUpdate() {
      this.isEditing = false
    },
    deleteSensor() {
      this.$emit("deleteSensor", this.editSensor)
    },
  },
})
</script>
