<script setup lang="ts">
import { Ref, ref } from "vue";
import draggable from "vuedraggable";
import CustomInput from "./CustomInput.vue";

const colList: Ref<{ cid: number }[]> = ref([]); // 拼图的宽有几格
const rowList: Ref<{ id: number }[]> = ref([{ id: 1 }]); // 拼图的总格子数
const colWidht = ref(); // 拼图里每个小格子的宽度
let copyRowList: number[] = []; // 用于对比是否完成拼图的数组

// 设置拼图格子宽高
const setPuzzleLength = (formData: { width: number; height: number }) => {
  // 重置宽高数组
  rowList.value = [];
  colList.value = [];
  copyRowList = [];
  // 计算出总的格子数
  const h = formData.width * formData.height;
  // 设置拼图总格子数组
  for (let i = 0; i < h; i++) {
    rowList.value.push({ id: i + 1 });
    copyRowList.push(i + 1);
  }
  // 打乱排序
  rowList.value.sort((a, b) => {
    return Math.random() > 0.5 ? -1 : 1;
  });
  // 设置拼图宽的格子数组
  for (let i = 0; i < formData.width; i++) {
    colList.value.push({ cid: i + 1 });
  }
  // 设置拼图里每个小格子的宽度
  colWidht.value = `calc((100% - ${h}px) / ${colList.value.length})`;
};

setPuzzleLength({ width: 3, height: 3 });
// 小格子的移动事件
const handleMovePuzzle = (evt: any, originalEvent: any) => {
  rowList.value = evt.relatedContext.list;
};

// 小格子移动完成后的事件
const handleEndStatus = () => {
  const t = rowList.value.reduce((prve, cur) => {
    return prve + cur.id;
  }, "");
  if (copyRowList.join("") === t) {
    setTimeout(() => {
      alert("拼图成功！！！");
    }, 100);
  }
};
</script>

<template>
  <CustomInput @onOk="setPuzzleLength" />
  <div class="draggable-box">
    <draggable
      class="row"
      item-key="id"
      :list="rowList"
      :animation="100"
      :forceFallback="true"
      :move="handleMovePuzzle"
      @end="handleEndStatus"
    >
      <template #item="{ element }">
        <div class="col">{{ element.id }}</div>
      </template>
    </draggable>
  </div>
</template>

<style scoped>
.row {
  display: flex;
  flex-wrap: wrap;
  width: 90vw;
  gap: 3px;
}
.col {
  width: v-bind(colWidht);
  height: 100px;
  background-color: gray;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
