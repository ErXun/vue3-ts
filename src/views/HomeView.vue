<template>
  <div class="home-view">
    <router-link :to="'/map'">Map</router-link>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref } from "vue";

export default defineComponent({
  name: "HomeView",
  components: {},
  setup() {
    return {};
  },
});
</script>

<style lang="less" scoped>
.home-view {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
