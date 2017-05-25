---
title: Vue.js 中的简单父子组件通信
date: 2017-03-16 15:16:46
tags: Vue.js
---

## 参考

> [Vue.js Components](https://vuejs.org/v2/guide/components.html)

## 实战基础

在 Vue.js 中，组件实例的作用域是孤立的，所以说在父组件中不能直接引用子组件的，反之亦然。Vue.js 提供了专门的传递数据方式，父组件通过 props 向下传递数据给子组件，子组件通过 events 给父组件发送消息。如图

![](http://7xoxnz.com1.z0.glb.clouddn.com/props-events.png)
<!-- more -->

## 错误示范

``` html
<!-- Parent.vue -->
<template>
  <div>
    <child :text="text"></child>
    <div>{{text}}</div>
  </div>
</template>

<script>
import { child } from '@/components';

export default {
 components: {
   child
 },
 data() {
   return {
     text: 'default'
   };
 }
}
</script>
```

``` html
<!-- child.vue -->
<template>
  <div>
    <input type="text" v-model="text">
  </div>
</template>

<script>
export default {
  props: {
    text: {
      type: String
    }
  }
}
</script>
```

上面这段代码中，父组件将初始 text 值传给子组件作为 prop，这个没有问题。但是当我们修改子组件内容时，直接修改了父组件的 text 值，有违 vue 的设计理念，因此在 console 中抛出异常：`[Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "text" `

下面来看看正确的例子。

## 简单例子

这个实例包含了 props down 和 events up。初始化时，父组件指定 input 的 text prop 专递给子组件；text 值输入时被修改后，子组件再回传 computed 的 _text 值给父组件更新 text 值。

``` html
<!-- Parent.vue -->
<template>
  <div>
    <child :text="text" @changeText="changeText"></child>
    <div>{{text}}</div>
  </div>
</template>

<script>
import { child } from '@/components';

export default {
  components: {
    child
  },
  data() {
    return {
      text: 'default'
    };
  },
  methods: {
    changeText(text) {
      this.text = text;
    }
  }
}
</script>
```

``` html
<!-- child.vue -->
<template>
  <input type="text" v-model="_text" @input="$emit('changeText',$event.target.value)">
</template>

<script>
export default {
  props: {
    text: {
      type: String
    }
  },
  computed: {
    _text() { return this.text; }
  }
}
</script>
```

官方文档中也有一个[类似的例子](https://vuejs.org/v2/guide/components.html#Form-Input-Components-using-Custom-Events)。官方的写法父组件是用的 v-model 属性，而我是将 v-model 语法糖进行了拆解，实际实现的效果都是相同的，但我认为拆解后更加灵活，因为使用 v-model 的话，只能绑定 value 作为 prop 的 key 值了。

## 进阶例子

如果子组件中有两个 input ，并且 input 值需要绑定。这时候，父组件用两个 v-model 属性肯定是行不通的。把 v-model 语法糖拆解开来，这个问题就得到解决了。

``` html
<!-- Parent.vue -->
<template>
  <div>
    <child
      :text="text"
      :number="number"
      @changeText="changeText"
      @changeNumber="num=>{number=num}">
    </child>
    <div>text: {{text}}, number: {{number}}</div>
  </div>
</template>

<script>
import { child } from '@/components';

export default {
  components: {
    child
  },
  data() {
    return {
      text: 'default',
      number: 12
    };
  },
  methods: {
    changeText(text) {
      this.text = text;
    }
  }
}
</script>
```

``` html
<!-- child.vue -->
<template>
  <div>
    <input type="text" v-model="_text" @input="$emit('changeText',$event.target.value)">
    <input type="number" v-model="_number" @input="$emit('changeNumber',$event.target.value)">
  </div>
</template>

<script>
export default {
  props: {
    text: {
      type: String
    },
    number: {
      type: Number
    }
  },
  computed: {
    _text() { return this.text; },
    _number() { return this.number; }
  }
}
</script>
```

## 更复杂的情况
大型应用或是更复杂的情况，要考虑使用 Vuex 来做专门的状态管理
