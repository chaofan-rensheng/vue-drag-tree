<template>
  <div class="container">
    <el-tree
      :data="data"
      node-key="id"
      default-expand-all
      highlight-current
      draggable
      :allow-drop="allowDrop">
    </el-tree>
  </div>
</template>

<script>
export default {
  data () {
    return {
      data: [{
        id: 'all',
        label: '全部',
        children: [{
          id: 1,
          label: '一级 1',
          children: [{
            id: 4,
            label: '二级 1-1',
            children: [{
              id: 9,
              label: '三级 1-1-1'
            }, {
              id: 10,
              label: '三级 1-1-2'
            }]
          }]
        }, {
          id: 2,
          label: '一级 2',
          children: [{
            id: 5,
            label: '二级 2-1'
          }, {
            id: 6,
            label: '二级 2-2'
          }]
        }, {
          id: 3,
          label: '一级 3',
          children: [{
            id: 7,
            label: '二级 3-1'
          }, {
            id: 8,
            label: '二级 3-2',
            children: [{
              id: 11,
              label: '三级 3-2-1'
            }, {
              id: 12,
              label: '三级 3-2-2'
            }, {
              id: 13,
              label: '三级 3-2-3'
            }]
          }]
        }]
      }],
      defaultProps: {
        children: 'children',
        label: 'label'
      }
    }
  },
  methods: {

    getMaxFloor (value) {
      let maxFloor = 0

      for (var i = 0; i < value.length; i++) {
        let currentFloor = 0
        if (!value[i].children || value[i].children === 0) return 0
        let children = value[i].children
        currentFloor += 1
        recursion(children)

        function recursion (children) {
          children.map(item => {
            if (item.children && item.children.length > 0) {
              currentFloor++
              recursion(item.children)
            } else {
              maxFloor = currentFloor > maxFloor ? currentFloor : maxFloor
            }
          })
        }
      }
      return maxFloor
    },

    allowDrop (draggingNode, dropNode, type) {
      // tree是否允许推拽的方法 return true 可以推拽  否则false
      let dropNodeLevel = dropNode.level // 推拽的哪里的当前层级
      let draggingNodeLevel = this.getMaxFloor([draggingNode.data]) // 推拽元素有几层

      if (dropNode.data.id === 'all') {
        // 判断推拽不能超出最外层
        return false
      } else {
        if (dropNodeLevel + draggingNodeLevel >= 4) {
          // 判断两者加起来是否超出规定的层数
          return false
        } else {
          return true
        }
      }
    }
  }
}
</script>
<style>
  .container{
    width: 300px;
    margin: 100px auto;
  }
</style>
