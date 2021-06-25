<template>
  <div class="reflash">
    {{ msg }}
  </div>
</template>
<script>
import { defineComponent } from "vue";
export default defineComponent({
  name: "listHead",
  props: {
    msg: String,
  },
  setup: (props) => {
    return {};
  },
});
</script>

<style lang="less">

</style>
