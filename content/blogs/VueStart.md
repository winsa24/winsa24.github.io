---
title: Vue Start
date: 2021-03-26

description: 
categories: [Front-end Develop]
tags: [Vue]

draft: false
enableDisqus : true
enableMathJax: false
disableToC: false
disableAutoCollapse: true

---
# Vue Start
> 要开始认真学习前端了。。听说复盘很重要，随手记录下。。

> **本机环境**：macOS 11.2  

## 安装Vue
控制台
```npm install -g vue-cli  
```
报错-->
```
Error: EACCES: permission denied, access '/usr/local/lib/node_modules'
```
解决--> 
```
sudo chown -R <username>: /usr/local/lib/node_modules
```
(username需替换，查询控制台输入id -un)

## 新建项目
cd 进项目存储目录
```
Vue init webpack <my-project>(项目名称)
```
然后会出一堆输入或者选项，比如Project name, description...看着随便填吧，我是一路Yes了

创建好后
```
cd (项目名称)
npm run dev
```

显示：Your application is running here: http://localhost:8080 ===> **成功**

> **Ref:** <https://www.jianshu.com/p/58920175e93d>

## TodoList

> 列举Task   
```
<ul>
      <li v-for="item in items">
        {{item.label}}
      </li>
</ul>
```
报错：   
```
[vue/require-v-for-key]
Elements in iteration expect to have 'v-bind:key' directives.eslint-plugin-vue
```
解决：  
```
<li v-for="item in items"  v-bind:key = "item"> 
```

> 实现简单todo list: <input> & <li>

> **Ref:** <https://www.ngui.cc/vuejs/show-4390.html>

******************
>第二天重启
命令行重新npm run dev (or npm start)
16个报错。。。
但localhost:8080 正常运行


>想搭一个更高级的，就去翻了下 [官网文档](https://cn.vuejs.org/v2/guide/installation.html)， 突然发现不推荐新手直接用vue-cli。。。anw

>自己尝试将直接写在App.vue里的代码挪到component里，但是失败了，所以搜了这个[教程](https://segmentfault.com/a/1190000013056647) 打算fo一下

>太复杂了，我还是先自己解决一个页面引多个components的问题吧。。。   
修改router --> index.js  
```
routes: [
    {
      path: '/',
      components: {
        HelloWorld,
        newcp
      }
    }
  ]
```
然后在App.vue里
```
<router-view name="HelloWorld"/>
<router-view name="newcp"/>
```

>然后想写一个简单的tabbar，实现页面间的转跳。但查阅了下资料发现“VUE就是单页应用”
我的测试：
可以在index.js中添加另一路径
```
{  path.....}
{
    path: '/abc',
    components: {
        HelloWorld,
        newcp,
        tabbar
    }
}
```
然后在tabbar.vue  component里用
```
<a href = "http://localhost:8080/#/abc">
```
硬指。 但是最终都会显示App.vue中的样式。   

>实现一个基本的**增删查改**的TodoList  
删除时，无法动态删除页面控件  
改用splice方法即可。官方文档解释：[数组更新检测](https://cn.vuejs.org/v2/guide/list.html#%E6%95%B0%E7%BB%84%E6%9B%B4%E6%96%B0%E6%A3%80%E6%B5%8B)

>显示   
其中isFinished属性用于实现点击完成
isSearched属性用于（查）操作
```
<div v-for="(item, index) in list" :key="index">
        <span v-bind:class="{'finished': item.isFinished, 'searched': item.isSearched}" v-on:click = "ChangeStatus(index)">{{item.name}}</span>
        <span>{{item.isFinished ? "Finished" : "Not finish" }}</span>
        <button v-on:click = "Delete(item)">Delete</button>
        <button v-on:click = "Edit(index)">Edit</button>
</div>
```
```
data () {
    return {
    ...
      list: [{
        name: 'Task1',
        isFinished: true,
        isSearched: false
      },
      {
        name: 'Task2',
        isFinished: false,
        isSearched: false
      }]
    }
  },
```
```
methods: {
    ...
    ChangeStatus: function (index) {
          this.list[index].isFinished = !this.list[index].isFinished
    },
    ...
}
```
>增
```
<input placeholder="Enter task name to add ..." v-model = "taskname" v-on:keyup.enter = "Add">
```
```
data () {
    return {
      taskname: '',
      ...
    }
  },
```
```
methods: {
    ...
    Add: function () {
      this.list.push({name: this.taskname, isFinished: false, isSearched: false})
      this.taskname = ''
    },
    ...
}
```
>删
```
methods: {
    ...
    Delete: function (item) {
      var i = 0
      while (this.list[i].name !== item.name) {
        i = i + 1
      }
      this.list.splice(i, 1)
    },
    ...
}
```
>改
```
<input placeholder="Enter new task name..." v-model= "editname">
```
```
methods: {
    ...
    Edit: function (index) {
          this.list[index].name = this.editname
    },
    ...
}

```
>查
```
<input placeholder="Enter task name to search ..." v-model = "searchname" v-on:keyup.enter = "Search">
```
```
methods: {
    ...
    Search: function () {
      for (var i = 0; i < this.list.length; i++) {
        if (this.list[i].name.search(this.searchname) !== -1) {
          this.list[i].isSearched = true
        } else {
          this.list[i].isSearched = false
        }
      }
    }
}
```

>注册组件
```
<hello v-for="(item, index) in list" v-bind:key="index" v-bind:item="item"></hello>
```
```
Vue.component('hello', {
  props: ['item', 'index'],
  template: `
  <div>
    <span v-bind:class="{'finished': item.isFinished, 'searched': item.isSearched}" v-on:click = "ChangeStatus(index)">{{item.name}}</span>
    <span>{{item.isFinished ? "Finished" : "Not finish" }}</span>
    <button v-on:click = "Delete(item)">Delete</button>
    <button v-on:click = "Edit(index)">Edit</button>
  </div>
  `
})
```
以上无法响应方法。但能动态接受数据（item & index)

***
解决 --->  转变思维：父组件传递整个数组（list)给子控件，子控件自己for，然后发送index / item回父组件，父组件进行处理

**子组件** （component/HelloWorld.vue）
```
<template>
<div>
  <div v-for="(item, index) in list" v-bind:key="index">
    <span v-bind:class="{'finished': item.isFinished, 'searched': item.isSearched}" v-on:click = "ChangeStatus(index)">{{item.name}}</span>
    <span>{{item.isFinished ? "Finished" : "Not finish" }}</span>
    <button v-on:click = "Delete(item)">Delete</button>
    <button v-on:click = "Edit(index)">Edit</button>
  </div>
</div>
</template>

<script>
export default {
  name: 'todo',                         //随便起，不重要
  props: ['list'],
  methods: {
    ChangeStatus: function (index) {
      this.$emit('changeS', index)
    },
    Delete: function (item) {
      this.$emit('delete', item)
    },
    Edit: function (index) {
      this.$emit('edit', index)
    }
  }
}
</script>
```
**父组件** (App.vue)
```
<template>
  <div id="app">
    <todo v-bind:list="list" v-on:changeS='ChangeStatus' v-on:delete='Delete' v-on:edit='Edit'></todo>
    <input placeholder="Enter task name to add ..." v-model = "taskname" v-on:keyup.enter = "Add">
    <input placeholder="Enter task name to search ..." v-model = "searchname" v-on:keyup.enter = "Search">
    ...
```
```
<script>
import HelloWorld from './components/HelloWorld.vue'
export default {
  name: 'App',
  components: {
    'todo': HelloWorld
  },
  ...
```
其他不用变   

> 最终效果：
![VueTodo](/images/blogs/VueTodo.gif)   


> **Ref:** <https://blog.csdn.net/kuangshp128/article/details/80680225> //比官方文档更清楚些   
<https://segmentfault.com/a/1190000012826671> 


