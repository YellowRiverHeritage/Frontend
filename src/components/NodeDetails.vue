<template>
  <div v-if="node" class="node-details">
    <h2>节点详情</h2>
    <h2>ID: {{ node.id }}</h2>
    <h2>标签: {{ node.label }}</h2>
    <table>
      <tbody>
        <tr v-for="(value, key) in filteredProperties" :key="key">
          <th>{{ translateKey(key) }}:</th>
          <td>{{ value }}</td>
        </tr>
        <tr v-if="node.properties.imageUrl">
          <th>{{ translateKey('imageUrl') }}:</th>
          <td><img :src="getImageUrl(node.properties.imageUrl)" alt="Node Image" style="max-width: 100px; max-height: 100px;"></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    node: {
      type: Object,
      default: () => ({ properties: {} })
    }
  },
  computed: {
    filteredProperties() {
      // 创建一个没有 imageUrl 的新对象
      const props = {...this.node.properties};
      delete props.imageUrl;  // 移除 imageUrl 属性
      return props;
    }
  },
  methods: {
    translateKey(key) {
      const translations = {
        name: '名称',
        birthday: '出生日期',
        courtesyName: '字',
        pseudonym: '号',
        era: '时代',
        occupation: '职业',
        works: '作品',
        nativePlace: '籍贯',
        description: '描述',
        imageUrl: '图片',
        period: '时期',
        city: '城市',
        title: '标题',
        author: '作者',
        verses: '诗句',
        creationBackground: '创作背景'
      };
      return translations[key] || key;  // 如果没有找到翻译，则返回原属性名
    },
    getImageUrl(imageFileName) {
      return require(`@/assets/${imageFileName}`);  // 根据项目结构调整路径
    }
  }
}
</script>

<style scoped>
.node-details {
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
  padding: 20px;
  margin-top: 20px;
}
.node-details h2 {
  color: #333;
  margin-bottom: 20px;
}
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}
th, td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}
</style>
