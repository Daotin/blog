# 85 Vue.js 组件精讲-vue组件精讲01

## vue组件分类

* 路由组件 

只用来还原设计稿的组件，基本不涉及到prop和自定义事件。

* 基础组件

也就是常说的库组件。（如时间选择器，带判断的输入框，提示弹框）

* 业务组件

经常和业务捆绑的比较紧密，在某个项目中可以复用，但是在其他项目无法通用。

## 作者最后说

> "Vue.js 组件开发，玩到最后还是在拼 JavaScript 功底。"

看来这个小册学完了，该进阶JavaScript了。

## 组件跨级通信 provide / inject

> provide 和 inject 主要为**组件库**使用。并不推荐直接用于业务组件中。_（不过建议归建议，如果你用好了，这个 API 会非常有用。）_

**这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。**

### 父传子

基本使用：假设有两个组件： A.vue 和 B.vue，B 是 A 的子组件。

```javascript
// A.vue
export default {
  provide: {
    name: 'Aresn'
  }
}

// B.vue
export default {
  inject: ['name'],
  mounted () {
    console.log(this.name);  // Aresn
  }
}
```

可以看到，在 A.vue 里，我们设置了一个 provide: name，值为 Aresn，它的作用就是将 name 这个变量提供给它的所有子组件。 而在 B.vue 中，通过 inject 注入了从 A 组件中提供的 name 变量，那么在组件 B 中，就可以直接通过 this.name 访问这个变量了，它的值也是 Aresn。这就是 provide / inject API 最核心的用法。

> 注意： provide提供的数据不是响应式的，修改了A.vue中name的值之后，B.vue里面name的值是不会变的。

**解决办法：** provide提供的数据直接是this。

```javascript
// A.vue
export default {
  data() {
      return {
          name: 'Aresn'
      }
  },
  provide() {
      return {
          appA: this
      }
  }
}

// B.vue
export default {
  inject: ['appA'],
  mounted () {
    console.log(this.appA.name);  // Aresn
  }
}
```

> 不好的是A.vue的data有很多不需要的数据，也会同时传给B.vue。

### 代替Vuex

如果把这个功能用在 app.vue 上会怎样呢？

由于项目中所有的组件（包含路由），它的父组件（或根组件）都是 app.vue，所以我们把整个 app.vue 实例的 data 对象通过 provide 对外提供。

```text
<template>
  <div>
    <router-view></router-view>
  </div>
</template>
<script>
  export default {
    provide () {
      return {
        app: this
      }
    }
  }
</script>
```

接下来，任何组件（或路由）只要通过 inject 注入 app.vue 的 `app` 的话，都可以直接通过 `this.app.xxx` 来访问 app.vue 的 `data`、`computed`、`methods` 等内容。

> app.vue 是整个项目第一个被渲染的组件，而且只会渲染一次（即使切换路由，app.vue 也不会被再次渲染），利用这个特性，很适合做一次性全局的状态数据管理，例如，我们将用户的登录信息保存起来。
>
> Tips：当 provide 用在根组件，跟`this.$root` 是一样的，但主要还是用在独立组件。

### 子传父

* 使用组件引用信息 `ref`。

## 混入 mixin

如果你的项目足够复杂，或需要多人协同开发时，在 app.vue 里会写非常多的代码，多到结构复杂难以维护。这时可以使用 Vue.js 的混合 mixins，将不同的逻辑分开到不同的 js 文件里。

混入是一个对象。相当于将一个组件分成多份，每一份负责一个功能点，**每一份都包含一个组件的所有属性**。

如下面就是一个混入对象：（是不是组件有的它都有）

```javascript
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}
```

一份份的mixin对象最后都合并到一个组件上，效果等同于把所有的代码都写到这个组件，不过用mixin结构会更清晰。

如定义一个获取用户信息的mixin为User.js：

```javascript
export default {
  data () {
    return {
      userInfo: null
    }
  },
  methods: {
    getUserInfo () {
      // 这里通过 ajax 获取用户信息后，赋值给 this.userInfo，以下为伪代码
      $.ajax('/user/info', (data) => {
        this.userInfo = data;
      });
    }
  },
  mounted () {
    this.getUserInfo();
  }
}
```

然后在 app.vue 中混合：

```text
<script>
  import mixins_user from './User.js';

  export default {
    mixins: [mixins_user],
    data () {
      return {

      }
    }
  }
</script>
```

这样，跟用户信息相关的逻辑，都可以在 user.js 里维护，或者由某个人来维护，app.vue 也就很容易维护了。

## $emit / $on

provide / inject / ref 是被动的父子组件的数据状态获取。比如父组件想获取子组件的数据需要使用$refs来获取，而不能子组件主动上报数据（也就是真正意义上的数据通信）。但是 $emit / $eventName 是可以的。

用法略。

> $emit / $eventName 只能父子组件间通信。如果想跨级组件间通信（不仅仅是父子之间），就用使用 $emit / $on（$eventName 和 $on 是不一样的），还需要一个空的 Vue 实例 `var eventHub = new Vue();` 来辅助（可以理解为一个事件处理中心），这样就可以跨组件通信。

## 组件通信终极方式：自定义的 findComponents 系列方法

它适用于以下场景：

* 由一个组件，向上找到最近的指定组件；
* 由一个组件，向上找到所有的指定组件；
* 由一个组件，向下找到最近的指定组件；
* 由一个组件，向下找到所有指定的组件；
* 由一个组件，找到指定组件的兄弟组件。

5 个不同的场景，对应 5 个不同的函数，实现原理也大同小异。

### 原理

5 个函数的原理，都是通过递归、遍历，找到指定组件的 `name` 选项匹配的组件实例并返回。

代码虽然自己写可能写不出来，但是理解起来就容易多了：

```javascript
// 由一个组件，向上找到最近的指定组件
function findComponentUpward (context, componentName) {
    let parent = context.$parent;
    let name = parent.$options.name;

    while (parent && (!name || [componentName].indexOf(name) < 0)) {
        parent = parent.$parent;
        if (parent) name = parent.$options.name;
    }
    return parent;
}
export { findComponentUpward };

// 由一个组件，向上找到所有的指定组件
function findComponentsUpward (context, componentName) {
    let parents = [];
    const parent = context.$parent;

    if (parent) {
        if (parent.$options.name === componentName) parents.push(parent);
        return parents.concat(findComponentsUpward(parent, componentName));
    } else {
        return [];
    }
}
export { findComponentsUpward };

// 由一个组件，向下找到最近的指定组件
function findComponentDownward (context, componentName) {
    const childrens = context.$children;
    let children = null;

    if (childrens.length) {
        for (const child of childrens) {
            const name = child.$options.name;

            if (name === componentName) {
                children = child;
                break;
            } else {
                children = findComponentDownward(child, componentName);
                if (children) break;
            }
        }
    }
    return children;
}
export { findComponentDownward };

// 由一个组件，向下找到所有指定的组件
function findComponentsDownward (context, componentName) {
    return context.$children.reduce((components, child) => {
        if (child.$options.name === componentName) components.push(child);
        const foundChilds = findComponentsDownward(child, componentName);
        return components.concat(foundChilds);
    }, []);
}
export { findComponentsDownward };

// 由一个组件，找到指定组件的兄弟组件
function findBrothersComponents (context, componentName, exceptMe = true) {
    let res = context.$parent.$children.filter(item => {
        return item.$options.name === componentName;
    });
    let index = res.findIndex(item => item._uid === context._uid);
    if (exceptMe) res.splice(index, 1);
    return res;
}
export { findBrothersComponents };
```

