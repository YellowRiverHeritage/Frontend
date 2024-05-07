<template>
  <el-card class="stats-card" header="图谱统计信息">
    <div class="table-container">
      <el-table :data="statsData" style="width: 100%;" border stripe>
        <el-table-column prop="key" label="统计项" width="100">
        </el-table-column>
        <el-table-column prop="value" label="数量" width="200">
        </el-table-column>
      </el-table>
    </div>
  </el-card>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      statsData: []
    };
  },
  mounted() {
    this.fetchStats();
  },
  methods: {
    fetchStats() {
      axios.get('http://localhost:8082/api/graph/stats')
        .then(response => {
          this.statsData = Object.keys(response.data).map(key => ({
            key: this.formatLabel(key),
            value: response.data[key]
          }));
        })
        .catch(error => console.error("Error fetching graph statistics:", error));
    },
    formatLabel(key) {
      const labels = {
        totalNodes: "总节点数",
        totalPoets: "诗人数",
        totalPoems: "诗作数",
        totalLocations: "地点数",
        totalRelationships: "总关系数"
      };
      return labels[key] || key;
    }
  }
}
</script>

<style scoped>
.stats-card {
  width: 100%;
  margin-bottom: 20px;
}

.table-container {
  height: 250px;  /* 固定高度，足以触发滚动条 */
  overflow: auto; /* 内容超出时显示滚动条 */
}
</style>
