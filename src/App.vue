<template>
  <div class="app">
    <div class="bg-gray">
      <HeaderBar @toggle="switchView" />
      <UserKeysPage v-if="state == 1" />
      <ReadingsPage v-if="state == 2" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import UserKeysPage from "./components/UserKeysPage.vue";
import ReadingsPage from "./components/ReadingsPage.vue"
import HeaderBar from "./components/HeaderBar.vue";
export default defineComponent({
  name: "App",
  // type inference enabled
  props: {
    name: String,
    msg: { type: String, required: true },
  },
  components: {
    HeaderBar,
    UserKeysPage,
    ReadingsPage,
  },
  data() {
    return {
      state: 1,
      msg: "Hi",
    };
  },
  methods: {
    switchView(num: number) {
      this.state = num;
    },
  },
});
</script>
