<template>
  <div class="wrap">
    <listItem class="listItem" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import listItem from "@/components/listItem.vue";
export default defineComponent({
  components: { listItem },
  setup: () => {
    return {
      name: "11",
    };
  },
});
</script>

<style lang="less" scoped>
@import "./assets/css/reset.css";

.listItem {
  height: 100%;
  background-color: gray;
}
</style>
>
