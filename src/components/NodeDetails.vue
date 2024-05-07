<template>
  <el-card class="node-details" header="节点详情：">
    <div v-if="node">
      <el-table
        :data="formattedNodeDetails"
        style="width: 100%;"
        :fit="false"
      >
        <el-table-column
          prop="key"
          label="属性"
          width="100px"
        >
        </el-table-column>
        <el-table-column
          prop="value"
          label="值"
          min-width="200px" 
        >
        </el-table-column>
      </el-table>
    </div>
    <div v-else>
      <p>请点击具体的节点。</p>
    </div>
  </el-card>
</template>

<script>
export default {
  props: {
    node: Object
  },
  computed: {
    formattedNodeDetails() {
      if (!this.node) return [];
      return Object.entries(this.node).map(([key, value]) => ({
        key,
        value
      }));
    }
  }
}
</script>

<style scoped>
.node-details {
  width: 100%;
  height: 500px;  
  overflow: auto;  
}

.node-details .el-table {
  width: 100%!important;
  min-height: 100%; 
  table-layout: fixed;
}

.node-details .el-table .cell {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>


