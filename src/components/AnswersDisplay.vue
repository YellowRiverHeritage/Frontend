<template>
  <el-card class="answers-display">
    <h2>搜索答案：</h2>
    <p>{{ answer }}</p>
  </el-card>
</template>

<script>
export default {
  props: {
    answer: String
  }
}
</script>

<style scoped>
.answers-display {
  width: 100%; /* 设置固定宽度 */
  height: 500px; /* 设置固定高度 */
  overflow: auto; /* 内容超出时允许滚动 */
}
</style>

