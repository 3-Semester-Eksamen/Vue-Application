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
                    Id
                  </th>
                  <th
                    scope="col"
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white">
                    Name
                  </th>
                  <th
                    scope="col"
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white">
                    E-mail
                  </th>
                  <th
                    scope="col"
                    class="py-3 px-6 text-xs font-medium tracking-wider text-left text-white">
                    Phone
                  </th>
                </tr>
              </thead>
              <tbody class="bg-gray-800 divide-y divide-gray-700">
                <tr
                  class="hover:bg-gray-700"
                  v-for="key in keyArray"
                  v-bind:key="key.id">
                  <td
                    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ key.id }}
                  </td>
                  <td
                    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ key.name }}
                  </td>
                  <td
                    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ key.email }}
                  </td>
                  <td
                    class="py-4 px-6 text-sm font-medium text-white whitespace-nowrap">
                    {{ key.phone }}
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
import { defineComponent } from "vue";
import type { keyType } from "../types/keyType";
import axios from "axios";
let keyArray: keyType[] = [];
export default defineComponent({
  data() {
    return {
      keyArray: keyArray,
    };
  },
  methods: {
    async getKeys() {
      let url = "https://localhost:7220/api/Keys";
      let response = await axios.get(url);
      this.keyArray = response.data;
    },
  },
  mounted() {
    this.getKeys();
  },
});
</script>
