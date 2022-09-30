<script setup lang="ts">
import { ref } from "vue";

const formData = ref({
  // 自定义设置拼图的宽高格子数
  width: 3,
  height: 3,
});
</script>

<template>
  <div class="input-box">
    <div class="form-item">
      <div class="label-item">宽：</div>
      <input type="number" v-model="formData.width" />
    </div>
    <div class="form-item">
      <div class="label-item">高：</div>
      <input type="number" v-model="formData.height" />
    </div>
    <button @click="$emit('onOk', formData)">确定</button>
  </div>
</template>

<style>
.input-box {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}
.form-item {
  display: flex;
  align-items: center;
}
.form-item + .form-item {
  margin-left: 20px;
  margin-right: 20px;
}
</style>
