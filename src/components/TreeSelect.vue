<template>
  <el-select
    ref="select"
    :value="value"
    placeholder="请选择"
    @visible-change="visibleChange"
    @clear="clear"
    clearable
    style="width: 100%"
  >
    <el-option class="option">
      <el-tree
        ref="tree"
        class="tree"
        :node-key="nodeKey"
        :data="data"
        :props="props"
        :default-expanded-keys="[value]"
        highlight-current
        :expand-on-click-node="false"
        @node-click="handleNodeClick"
      >
      </el-tree>
    </el-option>
  </el-select>
</template>

<script>
export default {
  name: 'TreeSelect',
  // v-model绑定设置
  model: {
    prop: 'value',
    event: 'change',
  },
  props: {
    // v-model绑定属性
    value: {
      type: [String, Number],
      default: '',
    },
    // 树结构数据
    data: {
      type: Array,
      default: function () {
        return [];
      },
    },
    // 树节点ID
    nodeKey: {
      type: [String, Number],
      default: 'id',
    },
    // tree属性配置
    props: {
      type: Object,
      default: function () {
        return {
          label: 'label',
          children: 'children',
        };
      },
    },
  },
  data() {
    return {};
  },
  watch: {
    value: {
      handler(val) {
        if (!this.isEmpty(this.data)) {
          this.init(val);
        }
      },
      immediate: true,
    },
    data: function (val) {
      if (!this.isEmpty(val)) {
        this.init(this.value);
      }
    },
  },
  methods: {
    isEmpty(val) {
      for (let key in val) {
        return false;
      }
      return true;
    },
    handleNodeClick(data) {
      this.$emit('change', data[this.nodeKey]);
      this.$refs.select.visible = false;
    },
    init(val) {
      if (!val) {
        this.$refs.tree && this.$refs.tree.setCurrentKey(null);
      }
      this.$nextTick(() => {
        this.$refs.tree.setCurrentKey(val);
      });
    },
    visibleChange(e) {
      if (e) {
        let selectDom = this.$refs.tree.$el.querySelector('.is-current');
        setTimeout(() => {
          this.$refs.select.scrollToOption({ $el: selectDom });
        }, 0);
      }
    },
    clear() {
      this.$emit('change', '');
    },
  },
};
</script>

<style lang="scss" scoped>
.option {
  height: auto;
  line-height: 1;
  padding: 0;
  background-color: #fff;
}

.tree {
  padding: 4px 20px;
  font-weight: 400;
}
</style>
