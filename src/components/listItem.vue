<template>
  <ul
    class="itemWrap"
    ref="wrap"
    @touchstart="touchstart"
    @touchmove="touchMove"
    @touchend="touchEnd"
    :style="{ paddingTop: paddingTop }"
  >
    <listHead class="listHead" :msg="headText"></listHead>
    <li class="item" v-for="item in listData" :key="item">
      {{ item }}
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import listHead from "./listHead.vue";
export default defineComponent({
  name: "listItem",
  components: { listHead },
  setup: () => {
    let startY: number;
    let moveY: number;

    const textList = ["下拉刷新", "松手刷新", "加载中", "刷新完成"];

    const baseList = new Array(50).fill(0).map((_, index) => index);

    let headText = ref(textList[0]);
    let wrap = ref(null);
    let paddingTop = ref("0px");
    let listData = ref(baseList);
    let refresh = ref(true);
    const touchstart = (e: any) => {
      let touch = e.changedTouches[0];
      refresh.value = e.target.offsetTop < touch.clientY;
      startY = touch.clientY;
    };

    const touchMove = (e: any) => {
      let touch = e.changedTouches[0];
      refresh.value = e.target.offsetTop + 25 < touch.clientY;
      if (refresh.value) {
        moveY = touch.clientY - startY;
        moveY < 600 ? (paddingTop.value = moveY + "px") : null;
        moveY > 40 ? (headText.value = textList[1]) : null;
      }
    };

    const touchEnd = () => {
      const loading = () => {
        paddingTop.value = "40px";
        headText.value = textList[2];
      };

      const loadingFinish = () => {
        paddingTop.value = "0px";
        headText.value = textList[0];
        moveY = 0;
      };
      if (moveY > 40) {
        loading();
        setTimeout(() => {
          headText.value = textList[3];
          loadingFinish();
          listData.value = listData.value.reverse();
        }, 2000);
      } else {
        loadingFinish();
      }
    };

    return {
      headText,
      wrap,
      listData,
      paddingTop,
      touchstart,
      touchMove,
      touchEnd,
    };
  },
});
</script>

<style lang="less">
.itemWrap {
  position: relative;
}
.item {
  border: 0.0625rem black solid;
  margin: 0.3125rem 0 0.3125rem 0;
  text-align: center;
}
.listHead {
  text-align: center;
  position: absolute;
  left: 50%;

  transform: translate(-50%, -150%);
}
</style>
