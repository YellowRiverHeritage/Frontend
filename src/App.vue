<template>
  <div id="app">
    <div class="network-container">
      <nodes-list @node-selected="handleNodeSelected" />
    </div>
    <div class="details-container">
      <node-details :node="selectedNode" />
    </div>
  </div>
</template>

<script>
import NodesList from './components/NodesList.vue';
import NodeDetails from './components/NodeDetails.vue';

export default {
  components: {
    NodesList,
    NodeDetails
  },
  data() {
    return {
      selectedNode: null
    };
  },
  methods: {
    handleNodeSelected(node) {
      this.selectedNode = node;
      if (node.label.includes('Person')) { // 检查是否为人物节点
      console.log("Name:", node.properties.name); // 打印名字
      console.log("Birth Date:", node.properties.birthDate); // 打印出生日期
      // 可以继续添加更多的属性打印
    }
    }
  }
}
</script>

<style>
#app {
  display: flex;
  height: 100vh;
}

.details-container {
  flex: 30%;
  overflow: auto;
  border-right: 1px solid #ccc;
}

.network-container {
  flex: 60%;
  overflow: hidden;
}

.answers-container {
  flex: 20%;
  border-left: 1px solid #ccc;
}
</style>
