---
title: Vue3 零基础入门教程：从核心概念到实战上手
published: 2026-03-15
tags: [Vue3, 前端开发, JavaScript, 零基础入门, 前后端分离]
category: 前端开发
draft: false
---

# Vue3 零基础入门教程：从核心概念到实战上手

Vue 是一款渐进式的 JavaScript 前端框架，也是目前国内最主流的前端开发框架之一。它以**上手门槛低、语法简洁、响应式能力强、组件化开发灵活**为核心优势，无论是做简单的页面交互，还是复杂的中后台管理系统，都能轻松适配。尤其对于.NET后端开发者，Vue + .NET Web API 是前后端分离开发的黄金组合，学习成本低，落地效率极高。

本文基于目前官方主推的 **Vue3 + <script setup> 组合式API** 语法，从环境搭建、核心概念、基础语法，到完整实战、新手避坑，零基础可全程跟着上手。

## 1. 前置知识与学习前提
学习本教程，你只需要掌握最基础的3项前端知识即可：
- HTML：了解基础标签的用法
- CSS：了解基础的样式设置
- JavaScript：了解变量、函数、对象、数组的基础用法

即使你是纯后端开发者，只有C#基础，也能快速理解Vue的核心逻辑，因为Vue的声明式开发、组件化思想，和面向对象编程有很多相通之处。

## 2. 环境搭建：两种入门方式
Vue提供了两种入门方式，分别对应「快速体验」和「正式项目开发」两种场景，新手推荐先从第一种零配置方式入手，快速理解核心概念。

### 2.1 零配置CDN快速上手（新手首选）
无需安装任何软件，只需创建一个HTML文件，通过script标签引入Vue，即可编写Vue代码，浏览器打开就能看到效果，零门槛体验所有核心特性。

完整的入门HTML模板如下，复制保存为`.html`后缀的文件，用浏览器打开即可运行：
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>Vue3 快速入门</title>
    <!-- 引入Vue3 CDN -->
    <script src="https://unpkg.com/vue@3/dist/vue.global.prod.js"></script>
</head>
<body>
    <!-- Vue挂载的容器 -->
    <div id="app">
        <!-- 插值表达式：渲染响应式数据 -->
        <h1>{{ message }}</h1>
        <button @click="changeMessage">点击修改内容</button>
    </div>

    <script>
        // 解构Vue的核心方法
        const { createApp, ref } = Vue;

        // 创建Vue应用实例
        createApp({
            // 组合式API写法，定义响应式数据和方法
            setup() {
                // 定义响应式数据
                const message = ref('Hello Vue3!');

                // 定义修改数据的方法
                const changeMessage = () => {
                    message.value = '你好，Vue3！';
                };

                // 暴露给模板使用的数据和方法
                return { message, changeMessage };
            }
        }).mount('#app'); // 将Vue实例挂载到id为app的容器上
    </script>
</body>
</html>
```

### 2.2 标准项目搭建（Vite，正式开发用）
如果要开发完整的前端项目，官方推荐使用 Vite 构建工具来创建 Vue 项目，它提供了极速的开发服务、热更新、打包优化等能力，是 Vue 项目的标准开发环境。
#### 搭建前提
先安装 Node.js 环境，版本要求 16.0 及以上，官网下载安装包一键安装即可，安装完成后可在终端输入node -v和npm -v验证是否安装成功。
#### 项目创建步骤
1. 打开终端（cmd / 终端工具），进入你想存放项目的文件夹，执行创建命令：
```bash
# npm 6.x 版本
npm create vite@latest vue-demo --template vue

# npm 7+ 版本（推荐）
npm create vite@latest vue-demo -- --template vue
```
2. 执行完成后，依次执行以下命令，安装依赖并启动开发服务：
```bash
# 进入项目文件夹
cd vue-demo
# 安装项目依赖
npm install
# 启动开发服务
npm run dev
```
3. 启动成功后，终端会显示本地服务地址（默认http://localhost:5173/），浏览器打开该地址，即可看到 Vue 的默认欢迎页面，项目创建完成。

### 核心项目结构说明

```plaintext
vue-demo
├── node_modules    # 项目依赖包，无需手动修改
├── public          # 静态资源文件夹，存放不会被编译的文件
├── src             # 开发核心目录，所有代码都写在这里
│   ├── assets      # 静态资源，存放图片、样式等
│   ├── components  # 公共组件文件夹
│   ├── App.vue     # 根组件，项目的入口组件
│   └── main.js     # 项目的入口文件，初始化Vue实例
├── index.html      # 项目的HTML入口文件
├── package.json    # 项目的依赖配置、脚本命令配置
└── vite.config.js  # Vite的配置文件
```
其中最核心的是.vue单文件组件，它是 Vue 项目的基本开发单元，一个.vue文件包含 3 个部分，对应 HTML、JS、CSS，结构清晰：
```vue
<template>
  <!-- 模板部分：写HTML结构，页面的展示内容 -->
  <div class="demo">
    <h1>{{ title }}</h1>
  </div>
</template>

<script setup>
// 脚本部分：写JS逻辑，定义响应式数据、方法、引入组件等
// <script setup> 是Vue3官方推荐的最简语法，自动暴露内容给模板使用
import { ref } from 'vue';

const title = ref('Vue单文件组件');
</script>

<style scoped>
/* 样式部分：写CSS样式，scoped表示样式只在当前组件生效 */
.demo h1 {
  color: #42b983;
}
</style>
```

## 3. Vue3 核心基础语法
### 3.1 声明式渲染与插值表达式
Vue 的核心特性之一是声明式渲染，我们只需要定义好数据，Vue 会自动把数据同步到页面上，无需手动操作 DOM。
最基础的渲染方式是插值表达式，语法是{{ 数据/表达式 }}，可以把响应式数据渲染到页面上，也支持简单的 JS 表达式。
示例：
```vue
<template>
  <div>
    <!-- 渲染基础数据 -->
    <p>姓名：{{ name }}</p>
    <p>年龄：{{ age }}</p>
    <!-- 支持简单表达式 -->
    <p>明年年龄：{{ age + 1 }}</p>
    <p>姓名大写：{{ name.toUpperCase() }}</p>
    <p>是否成年：{{ age >= 18 ? '是' : '否' }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// 定义响应式数据
const name = ref('张三');
const age = ref(20);
</script>
```

### 3.2 响应式数据：ref 与 reactive
Vue3 的响应式数据，核心通过ref和reactive两个 API 创建，只有响应式数据，修改时页面才会自动更新。
1. ref：定义基础类型 + 引用类型的响应式数据
ref 用于定义基础类型数据（字符串、数字、布尔值等），也可以定义对象、数组等引用类型。
- 特点：在`<script>`中修改数据，需要通过.value属性访问；在`<template>`模板中使用，无需写.value，Vue 会自动解包。
示例：
```vue
<script setup>
import { ref } from 'vue';

// 基础类型
const count = ref(0);
const message = ref('Hello');
const isShow = ref(true);

// 引用类型
const user = ref({
  name: '李四',
  age: 22
});

// 修改数据，必须加.value
const addCount = () => {
  count.value += 1;
  user.value.age += 1;
};
</script>
```
2. reactive：定义引用类型的响应式数据
reactive 专门用于定义`对象`、`数组`等引用类型的响应式数据，不能用于基础类型。
- 特点：修改数据时，无需加.value，直接通过属性修改即可。
示例：
```vue
<script setup>
import { reactive } from 'vue';

// 定义对象类型响应式数据
const user = reactive({
  name: '王五',
  age: 25,
  gender: '男'
});

// 定义数组类型响应式数据
const list = reactive(['苹果', '香蕉', '橙子']);

// 修改数据，无需.value
const updateUser = () => {
  user.age = 26;
  list.push('葡萄');
};
</script>
```
### 核心使用建议
- 基础类型数据（string/number/boolean），统一用ref
- 复杂对象 / 数组，用reactive
- 新手如果记不住区别，全用ref也可以，不会出错

### 3.3 常用内置指令
Vue 的指令是带有v-前缀的特殊属性，用于给 HTML 元素添加动态功能，是 Vue 模板的核心能力，以下是开发中最常用的指令。
1. v-bind：动态绑定属性
作用：给 HTML 标签的属性动态绑定响应式数据，比如src、class、style、disabled等。
- 简写语法：直接写:，代替v-bind:
示例：
```vue
<template>
  <div>
    <!-- 完整写法 -->
    <img v-bind:src="imageUrl" v-bind:alt="imageAlt">
    <!-- 简写写法（推荐） -->
    <img :src="imageUrl" :alt="imageAlt">

    <!-- 动态绑定class -->
    <div :class="{ active: isActive }">动态样式</div>

    <!-- 动态绑定禁用状态 -->
    <button :disabled="isDisabled">按钮</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';
const imageUrl = ref('https://vuejs.org/images/logo.png');
const imageAlt = ref('Vue Logo');
const isActive = ref(true);
const isDisabled = ref(false);
</script>
```
2. v-on：绑定事件监听
作用：给元素绑定事件，比如点击事件click、输入事件input等，实现用户交互。
- 简写语法：直接写@，代替v-on:
示例：
```vue
<template>
  <div>
    <p>计数：{{ count }}</p>
    <!-- 完整写法 -->
    <button v-on:click="addCount">加1</button>
    <!-- 简写写法（推荐） -->
    <button @click="subCount">减1</button>
    <!-- 直接内联写简单逻辑 -->
    <button @click="count = 0">重置</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';
const count = ref(0);

const addCount = () => {
  count.value++;
};
const subCount = () => {
  count.value--;
};
</script>
```

3. v-model：双向数据绑定
作用：实现表单元素和响应式数据的双向绑定 —— 数据变了页面自动更新，用户在表单里输入内容，数据也会自动更新。这是 Vue 的特色能力，大幅简化了表单处理的代码。
示例：
```vue
<template>
  <div>
    <p>你输入的内容是：{{ inputValue }}</p>
    <!-- 双向绑定输入框 -->
    <input type="text" v-model="inputValue" placeholder="请输入内容">

    <p>选中状态：{{ isChecked }}</p>
    <!-- 双向绑定复选框 -->
    <input type="checkbox" v-model="isChecked"> 同意协议
  </div>
</template>

<script setup>
import { ref } from 'vue';
const inputValue = ref('');
const isChecked = ref(false);
</script>
```
4. v-if /v-show：条件渲染
作用：根据条件控制元素的显示和隐藏，两者核心区别在于渲染机制不同。

|指令	|实现原理	|适用场景|
|-------|----------|--------|
|v-if	|条件为 false 时，直接销毁 / 不渲染该元素，条件为 true 时才创建渲染	|切换不频繁、条件复杂的场景，权限控制、一次性渲染|
|v-show	|无论条件真假，元素都会渲染，只是通过 CSS 的display: none控制显示隐藏	|切换非常频繁的场景，比如开关、弹窗显隐|

示例：
```vue
<template>
  <div>
    <button @click="isShow = !isShow">切换显示</button>

    <!-- v-if 示例 -->
    <div v-if="isShow">
      我是v-if控制的内容，条件为false时，DOM里不存在我
    </div>

    <!-- v-show 示例 -->
    <div v-show="isShow">
      我是v-show控制的内容，条件为false时，我只是被隐藏了，DOM里还在
    </div>

    <!-- 多条件分支 -->
    <div v-if="score >= 90">优秀</div>
    <div v-else-if="score >= 60">及格</div>
    <div v-else>不及格</div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
const isShow = ref(true);
const score = ref(85);
</script>
```
5. v-for：列表渲染
作用：遍历数组 / 对象，循环渲染列表元素，是开发中最常用的指令之一。
- 必须配合key属性使用，key必须是唯一值，用于 Vue 优化列表的渲染更新。
示例：
```vue
<template>
  <div>
    <h3>学生列表</h3>
    <ul>
      <!-- 遍历数组，item是每一项，index是索引 -->
      <li v-for="(item, index) in studentList" :key="item.id">
        序号：{{ index + 1 }} | 学号：{{ item.id }} | 姓名：{{ item.name }} | 年龄：{{ item.age }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const studentList = ref([
  { id: '001', name: '张三', age: 18 },
  { id: '002', name: '李四', age: 19 },
  { id: '003', name: '王五', age: 20 }
]);
</script>
```
### 3.4 计算属性 computed
作用：基于现有响应式数据，派生出新的数据，并且会缓存计算结果，只有依赖的数据源发生变化时，才会重新计算。适合用于需要复杂逻辑计算的渲染场景，比在模板里写表达式更清晰、性能更好。
示例：
```vue
<template>
  <div>
    <p>原始数组：{{ numberList }}</p>
    <p>大于10的数字：{{ filterNumberList }}</p>
    <p>总和：{{ total }}</p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const numberList = ref([5, 12, 8, 20, 3, 15]);

// 计算属性：过滤出大于10的数字
const filterNumberList = computed(() => {
  return numberList.value.filter(item => item > 10);
});

// 计算属性：计算数组总和
const total = computed(() => {
  return numberList.value.reduce((sum, item) => sum + item, 0);
});
</script>
```
#### computed vs 方法：
- 计算属性有缓存，依赖不变时，多次访问只会计算一次
- 方法没有缓存，每次调用都会重新执行
- 渲染场景优先用 computed，性能更好

### 3.5 侦听器 watch
作用：监听指定的响应式数据，当数据发生变化时，执行指定的回调函数，适合处理数据变化时的异步操作、复杂逻辑（比如数据变化时调用接口、执行副作用）。
基础示例：
```vue
<script setup>
import { ref, watch } from 'vue';

const count = ref(0);
const searchKeyword = ref('');

// 监听单个ref数据
watch(count, (newValue, oldValue) => {
  console.log(`count变化了，新值：${newValue}，旧值：${oldValue}`);
});

// 监听输入框，实现防抖搜索
watch(searchKeyword, (newValue) => {
  console.log(`用户输入了：${newValue}，执行搜索接口`);
  // 这里可以调用后端搜索接口
});
</script>
```
进阶用法：
```vue
<script setup>
import { reactive, watch } from 'vue';

const user = reactive({
  name: '张三',
  age: 18,
  info: {
    address: '北京'
  }
});

// 监听对象的某个属性
watch(() => user.age, (newValue) => {
  console.log(`年龄变化了：${newValue}`);
});

// 深度监听：监听整个对象，嵌套属性变化也能触发
watch(user, (newValue) => {
  console.log('user对象发生了变化', newValue);
}, { deep: true });

// 立即执行：页面初始化就执行一次回调
watch(() => user.name, (newValue) => {
  console.log(`姓名：${newValue}`);
}, { immediate: true });
</script>
```

## 4. 组件化开发基础
组件化是 Vue 的核心思想，它把一个复杂的页面，拆分成多个独立、可复用的小组件，每个组件负责自己的逻辑和样式，不仅代码结构清晰，还能大幅提升开发效率和可维护性。
比如一个后台管理页面，可以拆分为：顶部导航组件、侧边栏组件、表格组件、分页组件、弹窗表单组件等。
### 4.1 组件的创建与引入
在 Vue 项目中，一个.vue文件就是一个组件，我们可以在其他组件中引入并使用它。
示例步骤：
1. 在src/components文件夹下，创建一个StudentCard.vue组件：
```vue
<!-- src/components/StudentCard.vue -->
<template>
  <div class="student-card">
    <h3>学生信息卡片</h3>
    <p>姓名：张三</p>
    <p>年龄：18</p>
    <p>班级：计算机一班</p>
  </div>
</template>

<style scoped>
.student-card {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  width: 200px;
  margin: 10px;
}
</style>
```
2. 在App.vue根组件中，引入并使用这个组件：
```vue
<!-- src/App.vue -->
<template>
  <div>
    <h2>学生列表</h2>
    <!-- 直接使用组件，像HTML标签一样 -->
    <StudentCard />
    <StudentCard />
    <StudentCard />
  </div>
</template>

<script setup>
// 引入组件
import StudentCard from './components/StudentCard.vue';
</script>
```

### 4.2 父子组件通信
组件拆分后，组件之间需要传递数据、交互事件，最常见的就是父子组件之间的通信。
1. 父组件向子组件传值：Props
子组件通过defineProps声明接收的参数，父组件在使用子组件时，通过属性传递数据。
示例：
    1. 改造子组件StudentCard.vue，声明 props 接收数据：
```vue
<!-- src/components/StudentCard.vue -->
<template>
  <div class="student-card">
    <h3>学生信息卡片</h3>
    <p>姓名：{{ studentInfo.name }}</p>
    <p>年龄：{{ studentInfo.age }}</p>
    <p>班级：{{ className }}</p>
  </div>
</template>

<script setup>
// 声明props，定义接收的参数
const props = defineProps({
  // 基础类型参数，带类型校验
  className: {
    type: String,
    default: '未知班级' // 默认值
  },
  // 对象类型参数
  studentInfo: {
    type: Object,
    required: true // 标记为必填项
  }
});
</script>

<style scoped>
.student-card {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  width: 200px;
  margin: 10px;
  display: inline-block;
}
</style>
```

2. 父组件传递数据：

```vue
<!-- src/App.vue -->
<template>
  <div>
    <h2>学生列表</h2>
    <!-- 给子组件传递props数据 -->
    <StudentCard 
      className="计算机一班" 
      :studentInfo="student1" 
    />
    <StudentCard 
      className="计算机二班" 
      :studentInfo="student2" 
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import StudentCard from './components/StudentCard.vue';

// 定义要传递的数据
const student1 = ref({
  name: '张三',
  age: 18
});
const student2 = ref({
  name: '李四',
  age: 19
});
</script>
```
2. 子组件向父组件传值 / 触发事件：Emits
子组件通过defineEmits声明要触发的事件，通过emit函数向父组件发送事件和数据，父组件通过@事件名监听事件，接收数据。
示例：
    1. 改造子组件，添加按钮，触发事件给父组件：
```vue
<!-- src/components/StudentCard.vue -->
<template>
  <div class="student-card">
    <h3>学生信息卡片</h3>
    <p>姓名：{{ studentInfo.name }}</p>
    <p>年龄：{{ studentInfo.age }}</p>
    <p>班级：{{ className }}</p>
    <button @click="handleEdit">编辑</button>
    <button @click="handleDelete">删除</button>
  </div>
</template>

<script setup>
// 声明props
const props = defineProps({
  className: {
    type: String,
    default: '未知班级'
  },
  studentInfo: {
    type: Object,
    required: true
  }
});

// 声明要触发的事件
const emit = defineEmits(['edit', 'delete']);

// 触发编辑事件，把学生信息传给父组件
const handleEdit = () => {
  emit('edit', props.studentInfo);
};

// 触发删除事件
const handleDelete = () => {
  emit('delete', props.studentInfo.id);
};
</script>
```
2. 父组件监听子组件的事件：
```vue
<!-- src/App.vue -->
<template>
  <div>
    <h2>学生列表</h2>
    <StudentCard 
      className="计算机一班" 
      :studentInfo="student1"
      @edit="onEditStudent"
      @delete="onDeleteStudent"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import StudentCard from './components/StudentCard.vue';

const student1 = ref({
  id: '001',
  name: '张三',
  age: 18
});

// 监听编辑事件
const onEditStudent = (student) => {
  console.log('父组件收到编辑事件', student);
  alert(`要编辑的学生：${student.name}`);
};

// 监听删除事件
const onDeleteStudent = (id) => {
  console.log('父组件收到删除事件', id);
  alert(`要删除的学生ID：${id}`);
};
</script>
```

## 5. 完整入门实战：TodoList 待办事项应用
本节我们把前面所有的核心知识点串联起来，实现一个完整的待办事项应用，包含添加待办、标记完成、删除待办、筛选待办功能，代码可直接复制到 Vue 项目中运行。
完整代码
```vue
<template>
  <div class="todo-app">
    <h2>TodoList 待办事项</h2>

    <!-- 添加待办区域 -->
    <div class="add-area">
      <input 
        type="text" 
        v-model="newTodoText" 
        placeholder="请输入待办事项，按回车添加"
        @keyup.enter="addTodo"
      />
      <button @click="addTodo">添加</button>
    </div>

    <!-- 筛选区域 -->
    <div class="filter-area">
      <button 
        :class="{ active: filterType === 'all' }"
        @click="filterType = 'all'"
      >
        全部
      </button>
      <button 
        :class="{ active: filterType === 'active' }"
        @click="filterType = 'active'"
      >
        未完成
      </button>
      <button 
        :class="{ active: filterType === 'completed' }"
        @click="filterType = 'completed'"
      >
        已完成
      </button>
    </div>

    <!-- 待办列表 -->
    <ul class="todo-list">
      <li v-for="todo in filteredTodoList" :key="todo.id" :class="{ completed: todo.isCompleted }">
        <input 
          type="checkbox" 
          v-model="todo.isCompleted"
        />
        <span>{{ todo.text }}</span>
        <button class="delete-btn" @click="deleteTodo(todo.id)">删除</button>
      </li>
    </ul>

    <!-- 统计信息 -->
    <div class="stats">
      <span>总计：{{ todoList.length }} 条</span>
      <span>未完成：{{ activeCount }} 条</span>
      <span>已完成：{{ completedCount }} 条</span>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

// 待办输入框内容
const newTodoText = ref('');
// 筛选类型
const filterType = ref('all');
// 待办列表数据
const todoList = ref([
  { id: 1, text: '学习Vue3基础语法', isCompleted: true },
  { id: 2, text: '完成TodoList实战', isCompleted: false },
  { id: 3, text: '学习Vue组件化开发', isCompleted: false }
]);

// 计算属性：筛选后的待办列表
const filteredTodoList = computed(() => {
  switch (filterType.value) {
    case 'active':
      return todoList.value.filter(item => !item.isCompleted);
    case 'completed':
      return todoList.value.filter(item => item.isCompleted);
    default:
      return todoList.value;
  }
});

// 计算属性：未完成数量
const activeCount = computed(() => {
  return todoList.value.filter(item => !item.isCompleted).length;
});

// 计算属性：已完成数量
const completedCount = computed(() => {
  return todoList.value.filter(item => item.isCompleted).length;
});

// 添加待办
const addTodo = () => {
  const text = newTodoText.value.trim();
  if (!text) {
    alert('请输入待办事项内容');
    return;
  }
  // 新增待办项
  todoList.value.push({
    id: Date.now(), // 用时间戳作为唯一id
    text: text,
    isCompleted: false
  });
  // 清空输入框
  newTodoText.value = '';
};

// 删除待办
const deleteTodo = (id) => {
  todoList.value = todoList.value.filter(item => item.id !== id);
};
</script>

<style scoped>
.todo-app {
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}
.add-area {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}
.add-area input {
  flex: 1;
  padding: 8px 12px;
  font-size: 14px;
}
.add-area button {
  padding: 8px 20px;
  cursor: pointer;
  background: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
}
.filter-area {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}
.filter-area button {
  padding: 6px 12px;
  cursor: pointer;
  border: 1px solid #ccc;
  background: white;
  border-radius: 4px;
}
.filter-area button.active {
  background: #42b983;
  color: white;
  border-color: #42b983;
}
.todo-list {
  list-style: none;
  padding: 0;
  margin: 0 0 20px 0;
}
.todo-list li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  border-bottom: 1px solid #eee;
}
.todo-list li.completed span {
  text-decoration: line-through;
  color: #999;
}
.delete-btn {
  margin-left: auto;
  padding: 4px 8px;
  cursor: pointer;
  background: #ff4444;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 12px;
}
.stats {
  display: flex;
  gap: 20px;
  color: #666;
  font-size: 14px;
}
</style>
```

## 6. 新手高频易错点与避坑指南
### 6.1 ref 数据修改忘记加 .value
- 错误场景：在`<script>`中修改 ref 定义的数据，直接写count = 1，导致数据修改不生效，页面不更新。
- 正确做法：ref 定义的数据，在脚本中修改必须加.value，count.value = 1。
### 6.2 v-for 不写 key，或者用 index 作为 key
- 错误场景：写 v-for 时不写 key，或者直接用循环的 index 作为 key，导致列表更新时出现渲染错乱、数据错位的问题。
- 正确做法：必须给 v-for 绑定 key，key 必须是列表项的唯一标识（比如 id、唯一编码），禁止用 index 作为 key（尤其是列表有排序、删除、插入操作时）。
### 6.3 解构 reactive 数据，丢失响应式
- 错误场景：把 reactive 定义的对象解构出来使用，修改解构后的变量，原数据和页面都不更新，丢失响应式。
```javascript
const user = reactive({ name: '张三', age: 18 });
// 错误：解构后name和age变成普通变量，丢失响应式
let { name, age } = user;
```
- 正确做法：如果需要解构，使用toRefsAPI，保持响应式：
```javascript
import { reactive, toRefs } from 'vue';
const user = reactive({ name: '张三', age: 18 });
const { name, age } = toRefs(user);
// 修改时需要加.value
name.value = '李四';
```
### 6.4 v-if 和 v-for 同时用在同一个元素上
- 错误场景：在同一个标签上同时使用 v-if 和 v-for，Vue3 中 v-if 的优先级比 v-for 高，会导致 v-if 里访问不到 v-for 的变量，直接报错。
- 正确做法：把 v-for 写在外层的`<template>`标签上，内层写 v-if，或者先通过计算属性过滤好数据，再循环渲染。
### 6.5 直接修改 Props 数据
- 错误场景：在子组件中直接修改父组件传递过来的 props，导致 Vue 报警告，数据更新不符合预期。因为 props 是单向数据流，父组件传递的数据，子组件只能读，不能改。
- 正确做法：
    - 如果需要修改，先把 props 赋值给本地的 ref 响应式数据，再修改本地数据
    - 或者通过 emit 触发事件，让父组件修改原数据，符合单向数据流规范。
### 6.6 数组 / 对象直接修改索引 / 新增属性，页面不更新
- 错误场景：直接通过数组索引修改元素，或者给对象新增不存在的属性，数据变了，但页面不更新。
- 正确做法：
    - 数组修改：用数组的原生方法push、splice、filter等，或者重新给数组赋值
    - 对象修改：用Object.assign，或者重新给对象赋值，保证响应式更新。

## 7. 总结与进阶学习路线
本文覆盖了 Vue3 最核心的基础内容，从环境搭建、响应式基础、常用指令，到组件化开发、完整实战，掌握这些内容，你已经可以开发简单的前端页面和交互功能。
如果要开发完整的前后端分离项目，接下来的进阶学习路线如下：
- Vue Router：Vue 官方的路由管理器，用于实现单页应用的页面跳转、路由守卫等，是多页面项目的必备能力。
- Pinia：Vue 官方推荐的状态管理库，用于管理跨组件、跨页面的全局数据，比如用户信息、全局配置等。
- Axios：前端 HTTP 请求库，用于对接后端 API 接口（比如你的.NET Web API 接口），实现前后端数据交互。
- UI 组件库：比如 Element Plus、Ant Design Vue，提供了现成的表格、表单、弹窗、导航等组件，无需自己从零写样式，大幅提升开发效率，尤其适合开发后台管理系统。
- TypeScript：给 JavaScript 添加类型系统，和 C# 的强类型语法非常契合，能大幅提升代码的可维护性，减少线上 bug，是中大型项目的标配。
