<template>
  <div class="app-container">

    <div class="details-container">
      <graph-stats />
      <node-details :node="selectedNode"/>
    </div>
    <div class="network-container">
      <search-bar @search-submitted="handleSearch"/>
      <nodes-list @node-selected="handleNodeSelected"/>
    </div>
    <div class="answers-container">
      <answers-display :answer="searchAnswer"/>
    </div>
  </div>
</template>

<script>
import NodesList from './components/NodesList.vue';
import NodeDetails from './components/NodeDetails.vue';
import SearchBar from './components/SearchBar.vue';
import AnswersDisplay from './components/AnswersDisplay.vue';
import GraphStats from './components/GraphStats.vue';
import axios from 'axios';

export default {
  components: {
    NodesList,
    NodeDetails,
    SearchBar,
    AnswersDisplay,
    GraphStats
  },
  data() {
    return {
      selectedNode: null,
      searchAnswer: null
    };
  },
  methods: {
    handleNodeSelected(node) {
      this.selectedNode = node;
    },
    handleSearch(query) {
      const url = "http://localhost:8888";  // API的地址
      const data = {
        prompt: query,
        history: [],  // 根据需要初始化历史数据
        max_length: 2048,
        top_p: 0.7,
        temperature: 0.95
      };

      axios.post(url, data)
        .then(response => {
          if (response.data && response.status === 200) {
            this.searchAnswer = response.data.response;
          } else {
            this.searchAnswer = "发生了错误，请检查模型服务器。";
          }
        })
        .catch(error => {
          console.error("请求模型服务失败:", error);
          this.searchAnswer = "请求失败，请检查模型服务器。";
        });
    }

  }
}
</script>

<style>
.app-container {
  display: flex;
  justify-content: space-between;
}
.answers-container, .details-container {
  flex: 25%;
  padding: 20px;
}
.network-container {
  flex: 50%;
  padding: 20px;
}
</style>
