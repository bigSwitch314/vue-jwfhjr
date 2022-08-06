<template>
  <tree-select
    v-model="value"
    nodeKey="id"
    :data="treeData"
    :props="{ label: 'name' }"
  >
  </tree-select>
</template>

<script>
import TreeSelect from './TreeSelect.vue';
export default {
  name: 'HelloWorld',
  components: {
    'tree-select': TreeSelect,
  },
  data() {
    return {
      value: '',
      treeData: [
        {
          id: 1,
          name: '根节点',
          children: [
            {
              id: 11,
              name: '重庆',
            },
            {
              id: 12,
              name: '成都',
            },
          ],
        },
      ],
    };
  },
};
</script>

<style scoped></style>
