<template>
  <td class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap">
    {{ key.id }}
  </td>
  <td class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap">
    {{ key.name }}
  </td>
  <td class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap">
    {{ key.email }}
  </td>
  <td class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap">
    {{ key.phone }}
  </td>
  <td
    class="py-2 px-6 text-sm font-medium text-white whitespace-nowrap col-span-1 text-center">
    <button
      class="bg-red-700 text-gray-300 hover:bg-red-900 hover:text-white px-3 rounded-md text-sm font-medium py-2 w-20"
      @click="deleteKey">
      Delete
    </button>
  </td>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import type { PropType } from "vue";
import type { keyType } from "../../types/keyType";
export default defineComponent({
  data() {
    return {
      key: this.keyObj,
    };
  },
  props: {
    keyObj: {
      required: true,
      type: Object as PropType<keyType>,
    },
  },
  methods: {
    deleteKey() {
      this.$emit("deleteKey", this.key);
    },
  },
});
</script>
