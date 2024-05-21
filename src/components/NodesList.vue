<template>
  <div id="network" style="width: 100%; height: 800px;"></div>
</template>

<script>
import { Network } from 'vis-network';
import neo4j from 'neo4j-driver';

export default {
  data() {
    return {
      network: null,
      driver: null,
      session: null
    };
  },
  mounted() {
    this.connectToNeo4j();
    this.initializeNetwork();
  },
  beforeDestroy() {
    this.session && this.session.close();
    this.driver && this.driver.close();
  },
  methods: {
    connectToNeo4j() {
      this.driver = neo4j.driver('bolt://localhost:7687', neo4j.auth.basic('neo4j', 'JC20031230'));
      this.session = this.driver.session();
    },
    async initializeNetwork() {
      const container = this.$el;
      const { nodes, edges } = await this.fetchGraphData();

      const data = { nodes, edges };
      const options = {
  nodes: {
    shape: 'circle',
    size: 20, // 确保所有节点大小一致
    font: {
      size: 14,
      color: '#ffffff'
    },
    borderWidth: 2
  },
  edges: {
    arrows: 'to'
  }
};


      this.network = new Network(container, data, options);
      this.network.on('selectNode', (params) => {
        const nodeId = params.nodes[0];
        const node = this.network.body.nodes[nodeId].options;
        this.$emit('node-selected', node);
      });
    },
    async fetchGraphData() {
      const result = await this.session.run('MATCH (n)-[r]->(m) RETURN n, r, m');
      const nodes = new Map(); // Use a map to ensure each node is added only once
      const edges = [];

      result.records.forEach(record => {
        this.processNode(record.get('n'), nodes);
        this.processNode(record.get('m'), nodes);
        edges.push({
          from: record.get('n').identity.low,
          to: record.get('m').identity.low,
          label: record.get('r').type
        });
      });

      return { nodes: Array.from(nodes.values()), edges };
    },
    processNode(node, nodes) {
  if (!nodes.has(node.identity.low)) {
    let label = this.getLabel(node);
    let color = this.getNodeColor(node.labels);
    let properties = this.getProperties(node);

    nodes.set(node.identity.low, {
      id: node.identity.low,
      label: label,
      title: label, // 使用标签作为提示
      properties: properties,
      color: color,
      font: { color: '#ffffff' }, // 为了可读性设置字体颜色为白色
      shape: 'circle',
      size: 30 // 统一大小
    });
  }
},

getLabel(node) {
  if (node.labels.includes('Poem')) {
    return node.properties.title || '无标题';
  }
  return node.properties.name || '未知';
},

getNodeColor(labels) {
  if (labels.includes('Person')) return '#f57c00';
  if (labels.includes('Dynasty')) return '#ec407a';
  if (labels.includes('Place')) return '#42a5f5';
  if (labels.includes('Poem')) return '#66bb6a';
  return '#ddd';
},

getProperties(node) {
  let properties = {};
  for (let key in node.properties) {
    properties[key] = node.properties[key] || '未知';
  }
  return properties;
}
  }
}
</script>

<style>
#network {
  width: 100%;
  height: 800px;
}
</style>
