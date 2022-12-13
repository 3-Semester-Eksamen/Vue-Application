<template>
  <div class="absolute bottom-0 w-full text-center">
    <KanyeRestText />
  </div>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import KanyeRestText from "../components/organism/KanyeRestComp.vue";
export default defineComponent({
  setup() {},
  components: {
    KanyeRestText,
  },
});
</script>
