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
                  <th>
                    <button
                      class="bg-blue-700 text-gray-300 hover:bg-blue-900 hover:text-white px-3 rounded-md text-sm font-medium py-1 w-20"
                      @click="addNewKey">
                      Add new
                    </button>
                  </th>
                </tr>
              </thead>
              <tbody class="bg-gray-800 divide-y divide-gray-700">
                <tr
                  class="hover:bg-gray-700"
                  v-for="key in keyArray"
                  v-bind:key="key.id">
                  <UserKeysAssignedRow
                    :keyObj="key"
                    @deleteKey="deleteKey"
                    v-if="key.name !== `Unassigned`" />
                  <UserKeysUnassignedRow
                    :keyObj="key"
                    @deleteKey="deleteKey"
                    v-else
                    @updateKey="updateKey" />
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
import type { PropType } from "vue";
import type { keyType } from "../types/keyType";
import UserKeysAssignedRow from "../components/organism/UserKeysAssignedRow.vue";
import UserKeysUnassignedRow from "../components/organism/UserKeysUnassignedRow.vue";
import axios from "axios";
export default defineComponent({
  data() {
    return {
      keyArray: this.keys,
    };
  },
  methods: {
    async getKeys() {
      const url = "https://localhost:7220/api/Keys";
      const response = await axios.get(url);
      this.keyArray = response.data;
      this.$emit("updateKeys", this.keyArray);
    },
    async deleteKey(keyToDel: keyType) {
      const url = `https://localhost:7220/api/Keys/${keyToDel.id}`;
      const response = await axios.delete(url);
      await this.getKeys();
    },
    async updateKey(keyToUpd: keyType) {
      const url = `https://localhost:7220/api/Keys/${keyToUpd.id}`;
      const response = await axios.put(url, keyToUpd);
      await this.getKeys();
    },
    async addNewKey() {
      const url = `https://localhost:7220/api/Keys/`;
      const response = await axios.post(url, {
        id: 0,
        name: "string",
        email: "string",
        phone: "string",
      });
      await this.getKeys();
    },
  },
  mounted() {
    if (!this.keyArray.length) {
      this.getKeys();
      console.log("Getting keys");
    }
  },
  props: {
    keys: {
      required: true,
      type: Object as PropType<keyType[]>,
    },
  },
  components: {
    UserKeysAssignedRow,
    UserKeysUnassignedRow,
  },
});
</script>
