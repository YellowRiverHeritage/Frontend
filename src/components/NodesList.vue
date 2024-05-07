<template>
  <div id="network" style="width: 100%; height: 600px;"></div>
</template>

<script>
import { Network } from 'vis-network';
import axios from 'axios';

export default {
  data() {
    return {
      network: null,
      nodes: [],
      edges: []
    };
  },
  mounted() {
    axios.get('http://localhost:8082/api/graph/all')
      .then(response => {
        this.createGraph(response.data);
      })
      .catch(error => console.error("There was an error fetching the graph data:", error));
  },
  methods: {
    createGraph(data) {
      const container = document.getElementById('network');

      // Process poets and their poems
      data.poets.forEach(poet => {
        this.nodes.push({
          id: `poet-${poet.id}`,
          label: `${poet.name} (${poet.birthYear}-${poet.deathYear})`,
          title: poet.bio,
          group: 'poets'
        });

        poet.poems.forEach(poem => {
          this.nodes.push({
            id: `poem-${poem.id}`,
            label: poem.title,
            title: `${poem.title}: ${poem.content}`,
            group: 'poems'
          });
          this.edges.push({
            from: `poet-${poet.id}`,
            to: `poem-${poem.id}`,
            label: '创作'
          });

          poem.relatedLocations.forEach(location => {
            let locationId = `location-${location.id}`;
            if (!this.nodes.some(node => node.id === locationId)) {
              this.nodes.push({
                id: locationId,
                label: location.name,
                title: `${location.description}\nImportance: ${location.importance}`,
                group: 'locations'
              });
            }
            this.edges.push({
              from: `poem-${poem.id}`,
              to: locationId,
              label: '相关'
            });
          });
        });
      });

      const graphData = {
        nodes: this.nodes,
        edges: this.edges
      };

      const options = {
        nodes: {
          shape: 'dot',
          size: 20,
          font: {
            size: 14
          }
        },
        edges: {
          arrows: 'to'
        },
        layout: {
          hierarchical: {
            enabled: false
          }
        },
        physics: {
          enabled: true
        }
      };

      this.network = new Network(container, graphData, options);
      this.network.on("select", params => {
        if (params.nodes.length > 0) {
          const selectedNode = this.nodes.find(node => node.id === params.nodes[0]);
          this.$emit('node-selected', selectedNode);
        }
      });
    }
  }
}
</script>

<style scoped>
#network {
  width: 100%;
  height: 600px;
}
</style>
