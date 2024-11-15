+++
date = '2024-11-13T20:05:21+08:00'
draft = true
title = '响应式原理（简化）'
+++

初次发布于[我的个人文档](https://colablack.github.io/)

本文简要介绍一下如何实现一个简化版的类vue的响应式。

### 1.假装不知道响应式

如果我们不知道vue等响应式框架，那么又该如何手动实现类似的功能呢？

先来看这么一个简单的页面

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <title>My Program</title>
  </head>
  <body>
    <h1 id="data1"></h1>
    <p id="data2"></p>
    <p id="data3"></p>
  </body>
</html>

```

现在，如果这里的三个data需要变动，与此同时我们还希望页面进行所谓的“重新渲染”即让页面也跟着数据的变动变化应该怎么做呢？

首先，要写js代码让这几个标签有内容对吗？

只需要封装这三个函数即可

```javascript
      const showData1 = (data) => {
        document.getElementById("data1").innerHTML = data.data1;
      };
      const showData2 = (data) => {
        document.getElementById("data2").innerHTML = data.data2;
      };
      const showData3 = (data) => {
        document.getElementById("data3").innerHTML = data.data3;
      };
```

对于旧数据，只需要调用函数传入即可，例如

```javascript
      const showData1 = (data) => {
        document.getElementById("data1").innerHTML = data.data1;
      };
      const showData2 = (data) => {
        document.getElementById("data2").innerHTML = data.data2;
      };
      const showData3 = (data) => {
        document.getElementById("data3").innerHTML = data.data3;
      };
      const data = {
        data1: "Hello World!",
        data2: "This is a paragraph.",
        data3: "This is another paragraph.",
      };
      showData1(data);
      showData2(data);
      showData3(data);
```

但是接下来如果要更改data1，你会发现页面并不会变化，除非你重新调用showData1。

例如，这段代码就实现了3秒后变化data1并使得页面发生变化。

```javascript
      const showData1 = (data) => {
        document.getElementById("data1").innerHTML = data.data1;
      };
      const showData2 = (data) => {
        document.getElementById("data2").innerHTML = data.data2;
      };
      const showData3 = (data) => {
        document.getElementById("data3").innerHTML = data.data3;
      };
      const data = {
        data1: "Hello World!",
        data2: "This is a paragraph.",
        data3: "This is another paragraph.",
      };
      showData1(data);
      showData2(data);
      showData3(data);
      setTimeout(() => {
        data1 = "Welcome to my program.";
        showData1(data1);
      }, 3000);
```

### 2.针对特殊变量实现响应式

所以手动实现响应式的关键，就是在改变变量的时候再次调用showData函数。

然而每次都手动去调用肯定是很麻烦而且不优雅的。

很容易想到，只要重载对应变量的set方法就可以了。

```javascript
      const showData1 = (data) => {
        document.getElementById("data1").innerHTML = data.data1;
      };
      const showData2 = (data) => {
        document.getElementById("data2").innerHTML = data.data2;
      };
      const showData3 = (data) => {
        document.getElementById("data3").innerHTML = data.data3;
      };
      const data = {
        _data1: "Hello World!", // 使用一个下划线前缀来存储data1实际的值
        data2: "This is a paragraph.",
        data3: "This is another paragraph.",
      };
      Object.defineProperty(data, "data1", {
        get: function () {
          return this._data1; // 使用存储的实际值
        },
        set: function (value) {
          this._data1 = value; // 更新存储的实际值
          showData1(value); // 调用showData1来更新页面上的内容
        },
      });
      showData1(data);
      showData2(data);
      showData3(data);
      setTimeout(() => {
        data.data1 = "Welcome to my program."; // 使用新设置的setter
      }, 3000);
```

这样就完成了对data1的封装。

类似地，可以自己手动完成对data2 data3的封装。

然而，这并不是什么好的选择，自己动手还是太累了。

### 3.尝试封装为一般化工具

所以我们要来尝试封装成一般化的工具！

我们来手写一个封装或者说观察函数，来观察这个对象，为这个对象的所有字段都重写get和set方法就可以了。

```javascript
      function watch(obj) {
        for (const key in obj) {
          let innerValue = obj[key];
          Object.defineProperty(obj, key, {
            get: function () {
              return innerValue;
            },
            set: function (value) {
              innerValue = value;
            },
          });
        }
      }
```

也就是这样。

但是接下来你会发现，我们不知道应该调用哪些函数了。

不妨回过头想想，手动实现的时候你是怎么知道要调用showData1这个函数的。

我们为什么不把三个showData函数全部调用一遍呢？

是不是因为showData1这个函数使用了data1这个变量啊，或者说就是这个函数调用了data1的get方法。

所以，我们应该先重写get方法，记录哪个函数使用了get方法。

那我怎么知道是哪个函数正在使用get方法呢？

解决方案是，让这个函数使用get方法前手动在全局变量上记录自己，然后get函数访问全局变量获取信息。

原本我们的调用是

```javascript
      showData1(data);
```

现在改为

```javascript
      showData1(data);
```

接着，data1的get方法只需要访问全局变量window.__func就知道谁正在调用get方法了。

```javascript
      function watch(obj) {
        for (const key in obj) {
          let innerValue = obj[key];
          let funcs = [];
          Object.defineProperty(obj, key, {
            get: function () {
              if (window.__func &&!funcs.includes(window.__func)) {
                funcs.push(window.__func);
              }
              return innerValue;
            },
            set: function (value) {
              innerValue = value;
            },
          });
        }
      }
```

这就实现了依赖收集。

然后呢，在有函数调用set方法的时候，只需要调用funcs里面的所有函数即可，也就是派发更新。

```javascript
      function watch(obj) {
        for (const key in obj) {
          let innerValue = obj[key];
          let funcs = [];
          Object.defineProperty(obj, key, {
            get: function () {
              if (window.__func &&!funcs.includes(window.__func)) {
                funcs.push(window.__func);
              }
              return innerValue;
            },
            set: function (value) {
              for (let i = 0; i < funcs.length; i++) {
                funcs[i]();
              }
              innerValue = value;
            },
          });
        }
      }
```

### 4.设计代理

但是，每次在调用get方法前还要自己手动设置全局变量还是太麻烦。如何把这个过程也自动化呢？

其实我们就是想加强调用了属性get方法的函数的功能，而由于我们做的是通用组件又不好直接修改函数本身。

这时，可以建一个新的对象或者函数，由它代理，或者说替代我们访问旧的函数或对象。

例如

```javascript
      function runFunc(func) {
        window.__func = func;
        func();
        window.__func = null;
      }
```

以后，用户想调用func就使用runFunc代理，由runFunc替用户访问func。现在只要用户用这个函数访问func并且设置了被watch的对象，那么就实现了响应式了。
