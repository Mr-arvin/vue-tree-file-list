# vue-tree-file-list
A vue component for tree structure supports adding treenode/leafnode, editing node's name and dragging.

Thank ParadeTo who is the origin author, I just add some new functions to it.

Here is the origin address : https://www.npmjs.com/package/vue-tree-list

# use
``npm install vue-tree-file-list``

```javascript
<button @click="addNode">Add Node</button>
<vue-tree-file-list @click="onClick" :model="data" default-tree-node-name="new node" default-leaf-node-name="new leaf"></vue-tree-file-list>
<button @click="getNewTree">Get new tree</button>
<pre>
  {{newTree}}
</pre>
...
import { VueTreeFileList, Tree, TreeNode } from '../src'
export default {
  components: {
    VueTreeFileList
  },
  data () {
    return {
      newTree: {},
      data: new Tree([
        {
          name: 'Node 1',
          id: 1,
          pid: 0,
          dragDisabled: true,
          children: [
            {
              name: 'Node 1-2',
              id: 2,
              isLeaf: true,
              pid: 1
            }
          ]
        },
        {
          name: 'Node 2',
          id: 3,
          pid: 0,
          dragDisabled: true
        },
        {
          name: 'Node 3',
          id: 4,
          pid: 0
        }
      ])
    }
  },
  methods: {
    addNode: function () {
      var node = new TreeNode({ name: 'new node', isLeaf: false })
      if (!this.data.children) this.data.children = []
      this.data.addChildren(node)
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
    }
  }
}
```

# props
**default-tree-node-name**
 
 Default name for new treenode.

**default-leaf-node-name**

Default name for new leafnode.

# events
**click**

```javascript
<vue-tree-file-list @click="onClick" ...
...
onClick(model) {
  console.log(model)
}
...
```

# Forbid dragging
Use `dragDisabled` to forbid dragging:
```javascript
data: new Tree([
  {
    name: 'Node 1',
    id: 1,
    pid: 0,
    dragDisabled: true,
  ...
```

