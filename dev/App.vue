<template>
    <div>
        <button @click="addNode">Add Node</button>
        <vue-tree-file-list @click="onClick" @delete="onDelete" @blur="onBlur" @focus="onFocus" @appendChild="onAppend" :is-file="false" :model="data" default-tree-node-name="NewNode" default-leaf-node-name="new leaf"></vue-tree-file-list>
        <button @click="getNewTree">Get new tree</button>
        <pre>
          {{newTree}}
        </pre>
    </div>
</template>
<script>
  // import { VueTreeFileList, Tree, TreeNode } from '../dist/vue-tree-file-list.min'
  import { VueTreeFileList, Tree, TreeNode } from './../src/index'
  export default {
    components: {
      VueTreeFileList,
    },
    data () {
      return {
        newTree: {},
        data: new Tree([{
        "id": 0,
        "isLeaf": false,
        "name": "root",
        "children": [
          {
            "id": 1,
            "isLeaf": false,
            "name": "Node 1",
            "pid": 0,
            "dragDisabled": true,
            "addTreeNodeDisabled": true,
            "addLeafNodeDisabled": true,
            "editNodeDisabled": true,
            "delNodeDisabled": true,
            "children": [
              {
                "id": 2,
                "isLeaf": true,
                "name": "Node 1-2",
                "pid": 1
              }
            ]
          },
          {
            "id": 3,
            "isLeaf": false,
            "name": "Node 2",
            "pid": 0,
            "disabled": true,
            "children": [
              {
                "id": "1002",
                "isLeaf": false,
                "name": "NewNode",
                "pid": 3,
                "children": [
                  {
                    "id": "1003",
                    "isLeaf": false,
                    "name": "NewNode",
                    "pid": "1002"
                  }
                ]
              }
            ]
          },
          {
            "id": 4,
            "isLeaf": false,
            "name": "Node 3",
            "pid": 0
          }
        ]
      }])
      }
    },
    methods: {
      addNode: function () {
        var node = new TreeNode({ name: 'new node', isLeaf: false })
        if (!this.data.children) this.data.children = []
        this.data.addChildren(node)
        console.log(node)
      },

      getNewTree: function () {
        var vm = this
        function _dfs (oldNode) {
          var newNode = {}

          for (var k in oldNode) {
            if (k !== 'children' && k !== 'parent') {
              newNode[k] = oldNode[k]
            }
          }

          if (oldNode.children && oldNode.children.length > 0) {
            newNode.children = []
            for (var i = 0, len = oldNode.children.length; i < len; i++) {
              newNode.children.push(_dfs(oldNode.children[i]))
            }
          }
          return newNode
        }

        vm.newTree = _dfs(vm.data)
      },

      onClick(model) {
        console.log(model)
      },
      onAppend(status) {
        // alert('appending')
        console.log(status)
      },
      onBlur(model) {
        console.log(model)
      },
      onFocus(model){
        console.log(model.name)
      },
      onDelete(data) {
        var clickModel = data.clickModel
        var node  = data.node
        clickModel.remove()
        while (node._props.model.name !== 'root') {
          node = node.$parent
        }
      }

    }
  }
</script>
