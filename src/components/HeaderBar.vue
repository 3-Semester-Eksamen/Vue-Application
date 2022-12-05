<template>
  <nav class="bg-gray-800">
    <div class="mx-auto max-w-7xl px-2">
      <div class="relative flex h-16 items-center justify-between">
        <div
          class="flex flex-1 items-center justify-center">
          <div class="flex items-center">
            <img class="h-8 w-auto pr-8" src="/locklogo.png" />
          </div>
          <div class="">
            <div class="flex space-x-4">
              <button
                class="text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium"
                @click="submitChangeState(1)"
                :class="{ activeButton: state === 1 }">
                Key List
              </button>
              <button
                class="text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium"
                @click="submitChangeState(2)"
                :class="{ activeButton: state === 2 }">
                Readings Page
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  setup() {},
  data() {
    return {
      state: 1,
    };
  },
  methods: {
    submitChangeState(num: number) {
      this.$emit("toggle", num);
      this.state = num;
    },
  },
});
</script>

<style scoped>
.activeButton {
  background-color: darkred !important;
}
</style>
