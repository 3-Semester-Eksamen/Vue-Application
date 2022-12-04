<template>
  <div class="app">
    <div class="">
      <HeaderBar @toggle="switchView" />
      <IndexPage v-if="state == 1" />
      <UserKeysPage v-if="state == 2" />
      <AdminsPage v-if="state == 3" />
      <ReadingsPage v-if="state == 4" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import AdminsPage from "./components/AdminsPage.vue";
import IndexPage from "./components/IndexPage.vue";
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
    IndexPage,
    UserKeysPage,
    AdminsPage,
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
      console.log("State: " + this.state);
    },
  },
});
</script>
