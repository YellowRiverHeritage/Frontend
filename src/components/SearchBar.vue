<template>
  <div class="search-bar">
    <el-input v-model="query" placeholder="请输入查询内容" @keyup.enter="submitSearch" clearable></el-input>
    <el-button @click="submitSearch" type="primary">搜索</el-button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      query: ''
    };
  },
  methods: {
    submitSearch() {
      this.$emit('search-submitted', this.query);
    }
  }
}
</script>

<style scoped>
.search-bar {
  display: flex;
  align-items: center;
  gap: 10px;
  width: 90%; /* 可以根据需要设置具体的px值 */
}
</style>

