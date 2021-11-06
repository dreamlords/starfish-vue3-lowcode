## 技术栈

- Vue3
- typescript
- webpack
- jsoneditor
- mitt
- element-plus

## 目录结构
- componments 公共组件
- formcomponents 表单组件
  - CheckBox 复选框
  - JsonEditor JSON编辑器
  - KeyValueConfig 单选框键值对配置
  - KeyValueConfigMult 多选框键值对配置
  - Radio 单选框
  - Switch 开关
  - Text 文本
  - TextArea 文本域
- pages 存放页面
  - Editer 编辑组件页面
    - components 编辑器相关组件
      - dynamicform 动态表单渲染(已经配置完成的渲染)
- styles 全局样式
- router 路由管理
- store 状态管理
- utils 全局公共方法
  - _ 公共方法
  - editMouse 编辑器鼠标拖拽和监听鼠标滚动事件
  - dynamicform 表单组件公共表单配置
  - shortcutKey 组件操作方法(复制，粘贴，删除)

## 更新

> 2021/9/26 更新[配置完善、路由新增、sass模块引入](https://github.com/fhx10012091/starfish-h5/commit/7ba16bd0d1f37f2b35b087456b963830277200e5)

> 2021/9/27 更新[编辑模块移动与拖拽和鼠标滑动联动，放大缩小](https://github.com/fhx10012091/starfish-h5/commit/fc60e180a71675afbe3973ca8b7aeff77a684740)

> 2021/9/28 引用vuedraggable库，完成内容拖拽，内容顺序拖拽功能

> 2021/9/29 完成动态表单大致框架，定义数据格式，通过vuex实现动态交互

> 2021/9/30 电脑中病毒重装系统，心态崩了

> 2021/10/2 动态表单模块完成单选框组件、文本域组件

> 2021/10/3 增加多选框，修复滚动配置动态表单模块(右边模块)中间也会滚动问题，通过监听鼠标移动的标签中的类是否在右边进行判断

> 2021/10/4 动态表单验证，表单预览，表单保存

> 2021/10/5 增加json编辑器表单组件

> 2021/10/6 修复编辑器鼠标拖动和放大缩小冲突问题，增加所有表单组件验证规则判断，显示条件判断，jsonEditor表单组件全屏功能，增加公共组件dialog

> 2021/10/7 优化页面交互和样式

> 2021/10/8 组件的复制与粘贴

> 2021/10/9 快捷键功能

> 2021/10/31 保存和预览功能的表单验证

> 2021/11/6 使用json快速创建表单和直接修改json达到更新效果

## 问题

- 鼠标监听与顺序拖拽冲突问题：https://editor.csdn.net/md/?articleId=120528792
- 表单拖拽到页面时，数据是引用类型，导致拖多个相同表单，修改一个时其他都会变化，监听拖拽到页面时数据变化，通过深拷贝的方式对数据进行重新赋值
- 动态表单组件相关配置中，JsonEditor编辑器组件非直接v-model修改data的数据，而是通过mounted时动态设置，导致配置多个表单组件，切换表单组件，component(vue 中的内置组件)渲染的JSON编辑器不会重新执行mounted，所以储存的data和item都是渲染第一个JsonEditor的，其他JsonEditor更新，所有的JsonEditor都会更新。解决方法：通过监听切换表单组件，变更时单独对JsonEditor进行重新渲染，与其他表单组件隔离开，数据变更是异步的，导致了把JsonEditor从false到true，默认直接是true，组件同样不会刷新，所以先变false,再执行nextTick后变成true
- 使用快捷键粘贴表单控件时，无法准确监听表单控件数组的变化，因为表单控件是引用类型，如果单纯的给数组增加一个对象，会影响复制的组件，所以使用了深拷贝方法，但是深拷贝会把响应式消除，所以无法准确监听，解决方法是，存储一个基础数据类型，表示数组的长度，通过监听数组的长度的变化也可以实现相同的功能
- 当保存表单数据和预览表单数据时，要遍历所有配置的表单，验证里面的内容是否合法，通过切换curControl的方式一个一个的验证。但是保存的问题很多，需要深度监听表单的新增或表单的内容更新，有可能陷入死循环，可以多定义一个变量来判断点击时保存是否需要再次遍历验证

### eslint 问题

- https://blog.csdn.net/weixin_43768107/article/details/120556194


### 打包问题

- 打包出来的静态资源直接在浏览器打开路径不对，在html中直接使用的是绝对路径，找不到资源

> 解决方法：vue.config.js配置publicPath区分环境，生产环境输出文件为./

- 打包结果找不到iconfont资源，因为直接使用是cdn链接，没写协议，直接打开默认协议为file，所以直接写死https