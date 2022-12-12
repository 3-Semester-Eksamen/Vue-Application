<template>
  <td class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap"
  v-if="isEditing">
    {{ key.id }}
  </td>
  <td class="py-4 px-6 text-sm font-medium whitespace-nowrap"
  v-if="isEditing">
    <input type="text" v-model="newName">
  </td>
  <td class="py-4 px-6 text-sm font-medium whitespace-nowrap"
  v-if="isEditing">
  <input type="text" v-model="newEmail">
  </td>
  <td class="py-4 px-6 text-sm font-medium whitespace-nowrap"
  v-if="isEditing"> 
  <input type="text" v-model="newPhone">
  </td>
  <td
    class="py-2 px-6 text-sm font-medium text-white whitespace-nowrap col-span-1 text-center"
    v-if="isEditing">
    <button
      class="bg-green-700 text-gray-300 hover:bg-green-900 hover:text-white px-3 rounded-md text-sm font-medium py-2 w-20"
      @click="confirmUpdate">
      Confirm
    </button>
  </td>
  <td class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap"
  v-if="!isEditing">
    {{ key.id }}
  </td>
  <td class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap"
  v-if="!isEditing">
    {{ key.name }}
  </td>
  <td class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap"
  v-if="!isEditing">
    {{ key.email }}
  </td>
  <td class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap"
  v-if="!isEditing"> 
    {{ key.phone }}
  </td>
  <td
    class="py-2 px-6 text-sm font-medium text-white whitespace-nowrap col-span-1 text-center"
    v-if="!isEditing">
    <button
      class="bg-blue-700 text-gray-300 hover:bg-blue-900 hover:text-white px-3 rounded-md text-sm font-medium py-2 w-20"
      @click="assignKey">
      Assign
    </button>
  </td>
</template>
<script lang="ts">
import { defineComponent } from "vue"
import type { PropType } from "vue"
import type { keyType } from "../../types/keyType"
export default defineComponent({
  data() {
    return {
      key: this.keyObj,
      isEditing: false,
      newName: "",
      newEmail: "",
      newPhone: ""
    }
  },
  props: {
    keyObj: {
      required: true,
      type: Object as PropType<keyType>,
    },
  },
  methods:{
    assignKey(){
        this.isEditing = !this.isEditing
    },
    confirmUpdate(){
      this.key.name = this.newName
      this.key.email = this.newEmail
      this.key.phone = this.newPhone
      this.$emit("updateKey", this.key)
      this.isEditing = !this.isEditing
    }
  }
})
</script>
