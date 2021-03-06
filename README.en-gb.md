<a href="./README.md">中文文档</a> | English

<a href="./ChangeLog.en.md">Change Log</a>

# simple-tree

#### This is a simple component of showing and handling tree data. You can install and use this component in your project by npm or yarn, or you can also setup and run this project to experience the component.

----

## Preview
![demo](./src/assets/tree-sample.gif)


## Using in your project

### Step 1
```
yarn add simple-vue-tree --save
```
or
```
npm install simple-vue-tree --save
```

### Step 2
In the entry file main.js, you need to import simple-vue-tree and the styles like:  
```
import 'simple-vue-tree'
import 'simple-vue-tree/dist/lib/simple-tree.css'

```

### Step 3
Then you can using this component by use tag 'simple-tree' in the vue template.  
```
  <simple-tree
    :treeData="treeData">
  <simple-tree/>
```
API is following, and you can also find simple usages in the directory 'src/componenets/HelloWorld.vue'.

#

### Tree props
#### treeData <font color="yellow">[Array]</font>
An array of tree node data like:
```
[
  {
    id: 1,
    title: 'node-1'
  },
  {
    id: 2,
    title: 'node-2',
    children: [
      {
        id: 3,
        title: 'node-2-1',
      }
    ]
  }
]
```

#### props <font color="yellow">[Object]</font>
The configuration options of node data. Sometimes your node data is not has same name key as default. Default keys are _id_, _title_, and _children_.  
You can use this prop to send your node data keys to the component. For example, your node data is like:
```
{
  id: 1,
  name: 'node-1'
}
```
You need use this prop like:
```
{
  title: 'name'
}
```
Then the key _name_ will be used as the default key _title_.

#### expand <font color="yellow">[Boolean]</font>
Show expand icon. Default value: true.

#### nodeClass <font color="yellow">[Object, Array, String]</font>
The class of node element.

#### draggable <font color="yellow">[Boolean]</font>
You can drag and drop tree nodes by adding a draggable attribute. Default value: false.

#### allowDrag <font color="yellow">[function]</font>
This function will be executed before dragging a node. If false is returned, the node can not be dragged.

params: 
 - data <font color="yellow">[Object]</font> : your node data Object.

#### allowDrop <font color="yellow">[function]</font>
This function will be executed when the a node is dragging over another node. If true is returned, you can choose a position to drop the node and the _tree-drop_ event will emitted and you can handle your tree data in the handle function. If false is returned, the _tree-drop_ event will not be emitted.

params: 
 - dragVNode <font color="yellow">[Object]</font> : the dragging node's virtual node. 
 - dropVNode <font color="yellow">[Object]</font> : the node's virtual node where dropped.
 - position <font color="yellow">[String]</font> : _before_, _after_, and _inner_ are possible values, which is the relative position of dragging node and drop node. 

#### dragNote <font color="yellow">[Object]</font>
This prop will change the drop position notify when you drag the node. You can customize the notify which the default value is 
```
{
  before: 'As previous node',
  after: 'As next node',
  inner: 'As child node'
}
```
#### indentLine <font color="yellow">[Boolean | Array]</font>
This prop is used to show a vertical line before the same level nodes.
Diffrent color lines will be showed before diffrent level nodes according to a color list.
If the value is true, a default color list is ['red', 'green', 'blue'].
And you can also set a customized list to replace the defaule list.

#### maxIndent <font color="yellow">[Number]</font>
This prop is used to control the indent distance of children nodes. If you don't set prop _indentLimit_, the indent distance only depands on _maxIndent_. Default value: 20.

#### indentLimit <font color="yellow">[Number]</font>
This prop is used to limit the max indent distance of the tree.

If there are many node levels, the node will overflow the tree box horizontally.And you can set this prop to avoid this unexpected effect.

Notice: if _indentLimit_ < 100, this prop will be regarded as percentage. In this instance, the value will be _treeWidth _ * _indentLimit_ / 100.

The indent width of each level will be _value_ / _levels_.

#### splitPage <font color="yellow">[Boolean]</font>
This prop is used to control whether the show style is split-page or not, with a default value _false_.

#### refreshPage <font color="yellow">[callback]</font>
A callback function will be called that when the show node changes. And the arguments will be a list which contains all the root nodes' brief information like { id: xxx, title: xxx }.

### Tree methods

| methods | introduce | args |
| --- | --- | --- |
| pushToShow | Used in _split-page_ mode, and call this function to show the children of the node you give | nodeData |
| jumpBack | Used in _split-page_ mode, and call this function to back to a root node of current node | the id of the node |

#

### Tree events

Notice: the return type 'vNode' means that the value is virtual dom of tree-node.
Some data of vNode is:

_nodeData_ : data of current node.

_parentData_ : data of current node's parent node. Mostly, _parentData_ is the refrence of your tree-node data. But there is a exceptive case when _parentData_ is the root of the tree. In this case, _parentData_ will be like:
```
{
  isRoot: true,
  children: [
    // ...
  ]
}
```
The property _isRoot_ is only for the 1st level nodes's parentData, and the property _children_ is the reference of your tree data. 

#### _content-click_
This event will be emited when the node content element is clicked.

- __return value__: (event, vNode)  
__vNode__: virtual dom of current tree node.

#### _content-double-click_
This event will be emited when the node content element is double clicked.

- __return value__: (event, vNode)  
__vNode__: virtual dom of current tree node.

#### _node-click_
This event will be emited when the node element is clicked. Though similar to _content-click_, it differs in taht it is fired when a chilren node element is clicked. In most cases, maybe _content-click_ satisfy your demand.

- __return value__: (event, vNode)  
__vNode__: virtual dom of current tree node.

#### _node-double-click_
This event will be emited when the node element is double clicked. Though similar to _content-double-click_, it differs in taht it is fired when a chilren node element is clicked.

- __return value__: (event, vNode)  
__vNode__: virtual dom of current tree node.

#### _expand-button-click_
This event will be fired when click the expanded button.

The following content is about tree node dragging event. Maybe _tree-drop_ is enough in most case except that you need customize the dragging effect.
#### _tree-drop_
- __return value__: (dragVNode, dropVNode, position)  
__dragVNode__: the dragging node's virtual dom.
__dropVNode__: the node's virtual dom where dropped.
__position__: the relative position, and the posiible values are _before_, _after_ and _inner_. 

#### _tree-drag-start_
- __return value__: (event, vNode)  
__vNode__: virtual dom of the dragging tree node.

#### _tree-drag-end_
- __return value__: (event, vNode)  
__vNode__: virtual dom of the dragging tree node.

#### _tree-drag-enter_
- __return value__: (event, vNode)  
__vNode__: virtual dom of the node where dropped.

#### _tree-drag-over_
- __return value__: (event, vNode)  
__vNode__: virtual dom of the node where dropped.

#### _tree-drag-position_
This event is fired when you choose a drop position. 
- __return value__: (event, position)  
__position__: virtual dom of the node where dropped.

#### _tree-drag-leave_
- __return value__: (event, vNode)  
__vNode__: virtual dom of the node where dropped.


---
## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Lints and fixes files
```
yarn run lint
```