# 上大海润vue开发规范-初稿

>为了公司内协同合作开发的便利性，整理出vue相关开发规范

## 一、强制性要求

### 1.以多个单词组成组件名

除了`app.vue根组件`之外，所有组件都已多个单词为命名方式，且每个单词首字母大写，使用时以小写单词加上'-'分割
例子：
`定义`
```javascript
export default {
  name: 'TodoList'
}
```
`使用`
```
<tode-list></todo-list>
```

### 2.组件data须以函数

除new Vue()之外的所有组件中，data的值必须是一个返回对象的函数
例子：
`定义`
```javascript
export default {
  data: {
    title: '上大海润'
  }
}
```

### 3.prop定义

prop的定义需要尽量详细，至少指定其类型
例子：
```javascript
export default {
  prop: {
    list: {
      type: String
    }
  }
}
```

### v-for键值设置

在v-for循环的组件中，总为v-for设置key，并且尽量避免使用index，而是使用数据中的id
例子：
```html
<div class="list"
  v-for="item in list"
  :key="item.id"
>
  {{item.title}}
</div>
```

### 避免v-if与v-for同时使用

- 一般我们在两种常见的情况下会倾向于这样做：
为了过滤一个列表中的项目 `(比如 v-for="user in users" v-if="user.isActive")`。在这种情形下，请将 users 替换为一个计算属性 (比如 `activeUsers`)，让其返回过滤后的列表。
- 为了避免渲染本应该被隐藏的列表 `(比如 v-for="user in users" v-if="shouldShowUsers")`。这种情形下，请将 v-if 移动至容器元素上 (比如 `ul, ol`)。

例子：
```html
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

### 为组件样式设置作用域

一般情况下，设置scoped属性即可，如遇大型工程，就需要考虑css moudles,同时在任何时候都需要添加一个组件或者app专属的前缀，以保护你的css

### 私有属性名

在插件、混入等扩展中始终为自定义的私有属性使用 $_ 前缀。并附带一个命名空间以回避和其它作者的冲突 (比如 $_yourPluginName_)。
例子：
```javascript
var myGreatMixin = {
  // ...
  methods: {
    $_myGreatMixin_update: function () {
      // ...
    }
  }
```

##可读性推荐

### 组件提取文件以及命名

将每个组件单独分成文件,同时使用PascalCase命名组件文件。

>components/
>|- TodoList.vue
>|- TodoItem.vue

### 基础组件名

应用特定样式和约定的基础组件 (也就是展示类的、无逻辑的或无状态的组件) 应该全部以一个特定的前缀开头，比如 Base、App 或 V。

>components/
>|- VButton.vue
>|- VTable.vue
>|- VIcon.vue

### 单例组件名

`只应该拥有单个活跃实例的组件应该以 The 前缀命名，以示其唯一性。`

这不意味着组件只可用于一个单页面，而是每个页面只使用一次。这些组件永远不接受任何 prop，因为它们是为你的应用定制的，而不是它们在你的应用中的上下文。如果你发现有必要添加 prop，那就表明这实际上是一个可复用的组件，只是目前在每个页面里只使用一次。

>components/
>|- TheHeading.vue
>|- TheSidebar.vue

### 紧密耦合的组件名

`和父组件紧密耦合的子组件应该以父组件名作为前缀命名。`

如果一个组件只在某个父组件的场景下有意义，这层关系应该体现在其名字上。因为编辑器通常会按字母顺序组织文件，所以这样做可以把相关联的文件排在一起。

>search/
>|-components/
>　|-SearchSidebar.vue
>　|- SearchSidebarNavigation.vue

### 组件名中的单词顺序

组件名应该以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。

>components/
>|- SearchButtonClear.vue
>|- SearchButtonRun.vue
>|- SearchInputQuery.vue

### 自闭合组件

在单文件组件、字符串模板和 JSX 中没有内容的组件应该是自闭合的——但在 DOM 模板里永远不要这样做。

>在单文件组件、字符串模板和 JSX 中
```
<MyComponent/>
```
>在 DOM 模板中
```
<my-component></my-component>
```

### 模板中的组件名大小写

对于绝大多数项目来说，在单文件组件和字符串模板中组件名应该总是 PascalCase 的——但是在 DOM 模板中总是 kebab-case 的。
建议使用PascalCase
```
<my-component></my-component>
```

### 完整单词的组件名

组件名应该倾向于完整单词而不是缩写

### Prop 名大小写

在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板和 JSX 中应该始终使用 kebab-case。

```javascript
props: {
  greetingText: String
}

<WelcomeMessage greeting-text="hi"/>
```

### 多个特性的元素

多个特性的元素应该分多行撰写，每个特性一行,利于阅读

```html
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
```
 
### 模板中简单的表达式

这里我们建议模版中不带任何表达式，全部用计算属性或方法

```
{{ normalizedFullName }}
```

```javascript
computed: {
  normalizedFullName: function () {
    return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1)
    }).join(' ')
  }
}
```

### 简单的计算属性

应该把复杂计算属性分割为尽可能多的更简单的属性。易于测试、易于阅读、更好的“拥抱变化”

```javascript
computed: {
  basePrice: function () {
    return this.manufactureCost / (1 - this.profitMargin)
  },
  discount: function () {
    return this.basePrice * (this.discountPercent || 0)
  },
  finalPrice: function () {
    return this.basePrice - this.discount
  }
}
```
### 指令缩写

用 : 表示 v-bind: 和用 @ 表示 v-on:

## 习惯性编码

1. 使用stylus,css样式中 不要`:`,`;`,`{}`
2. 代码缩进为2格


## 编辑器推荐

>VSCode

### 推荐插件

1. Auto Close Tag
2. Path Intellisense
3. Prettier
4. Vetur
5. language-stylus
6. eslint
7. cssrem