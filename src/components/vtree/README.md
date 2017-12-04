# Version: 2.0  
## API Document

**************************************
## English Document
**************************************
### Node Property
| Parameters | Description | Type | Optional values | Default value |
|---------- |-------- |---------- |---------- |---------- |
|title | node name | String | N | -
|expanded | node Expansion | Boolean | Y | false |
|checked | whether the node check box is selected | Boolean | Y | false |
|halfcheck | Whether the node is half optional (subordinate selected) | Boolean | Y | false |
|visible | is the node visible | Boolean | Y | false |
|selected | whether the node is selected | Boolean | Y | false |
|children | child nodes | Array[object] | Y | -

### Tree Property
| Parameters | Description | Type | Optional values | default value |
|---------- |-------- |---------- |---------- |---------- |
|data | tree Data Source | Array[object] | N | -
|multiple | turn on Check mode | Boolean | Y | false |
|tpl | custom templates | JSX | Y | False |
|halfcheck | turn on semi-select mode | Boolean | Y | select All |
|scoped | quarantine node Selected state | Boolean | Y | dalse |
|draggable | support drag? | Boolean | Y | dalse |
|dragafterexpanded | ro expand after dragging | Boolean | Y | true |

### method
| Method name | Description | Parameters |
|---------- |-------- |---------- |
| getSelectedNodes | returns an array of currently selected nodes | - |
| getCheckedNodes | returns the array of nodes selected by the current check box | - |
| searchNodes | customfilter:function/string |

**************************************
## 中文文档
**************************************

###  Node 属性
| 参数      | 说明    | 类型      | 可选值 | 默认值  |
|---------- |-------- |---------- |---------- |---------- |
|title     | 节点名称 | String | N | — |
|expanded |  节点是否展开 | Boolean | Y | false |
|checked |  节点复选框是否选中 | Boolean | Y | false |
|halfcheck |  节点是否为半选（下级被选中） | Boolean | Y | false |
|visible |  节点是否可见 | Boolean | Y | false |
|selected |  节点是否被选中 | Boolean | Y | false |
|children |  子节点 | Array[Object] | Y | — |

###  Tree 属性
| 参数      | 说明    | 类型      | 可选值 | 默认值  |
|---------- |-------- |---------- |---------- |---------- |
|data     | 树数据源 | Array[Object] | N | — |
|multiple |  开启复选模式 | Boolean | Y | false |
|tpl |  自定义模板 | JSX | Y | false |
|halfcheck |  开启半选模式 | Boolean | Y | 全选 |
|scoped |  隔离节点选中状态 | Boolean | Y | false |
|draggable | 是否支持拖拽 | Boolean | Y | false |
|dragAfterExpanded | 拖拽后展开   | Boolean | Y | true |

### 方法
| 方法名      | 说明    | 参数      |
|---------- |-------- |---------- |
| getSelectedNodes  | 返回目前被选中的节点所组成的数组 | - |
| getCheckedNodes  |返回目前复选框选中的节点组成的数组 | - |
| searchNodes  |搜索 | customFilter： function / String |

### How to use

Step1: install plugins
```
npm install\
  babel-plugin-syntax-jsx\
  babel-plugin-transform-vue-jsx\
  babel-helper-vue-jsx-merge-props\
  babel-preset-env\
  --save-dev

npm install vue-halower-tree --save
```
Step2： In your main.js
```
import VTree from 'vue-halower-tree'

Vue.use(VTree)
```
Step3: In your .babelrc
```
{
  "presets": ["env"],
  "plugins": ["transform-vue-jsx"]
}
```

### Demo

`Html`
```
  <v-tree ref='tree' :data='treeData' :multiple='true' :tpl='tpl' :halfcheck='true'/>
     <input type="text" v-model="searchword" />
    <button type="button" @click="search">GO</button>
```
`JS`
```
export default {
  name: 'HelloWorld',
  data () {
    return {
      searchword: '',
      treeData: [{
        title: 'First-level nodes',
        expanded: true,
        children: [{
          title: 'Level Two Node 1',
          expanded: true,
          children: [{
            title: 'Level Three node 1-1'
          }, {
            title: 'Level Three node 1-1'
          }, {
            title: 'Level Three node 1-1'
          }]
        }, {
          title: 'Level Two Node 2',
          children: [{
            title: "<span style='color: red'>Level Three node 2-1</span>"
          }, {
            title: "<span style='color: red'>Level Three node 2-2</span>"
          }]
        }]
      }]
    }
  },
  methods: {
    tpl (node, ctx) {
      let titleClass = node.selected ? 'node-title node-selected' : 'node-title'
      if (node.searched) titleClass += ' node-searched'
      return <span>
        <button style='color:blue; background-color:pink' onClick={() => this.$refs.tree.addNode(node, {title: 'Synchronous loading'})}>+</button>
      <span class={titleClass} domPropsInnerHTML={node.title} onClick={() => {
        ctx.parent.nodeSelected(ctx.props.node)
        console.log(ctx.parent.getSelectedNodes())
      }}></span>
      <button style='color:green; background-color:pink' onClick={() => this.asyncLoad(node)}>Asynchronous loading</button>
      <button style='color:red; background-color:pink' onClick={() => this.$refs.tree.delNode(node.parent, node)}>Delete</button>
      </span>
    },
    async asyncLoad (node) {
      this.$refs.tree.addNodes(node, await this.$api.demo.getChild())
    },
    search () {
      this.$refs.tree.searchNodes(this.searchword)
    }
  }
}
</script>

```