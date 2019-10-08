---
title: 'Object.prototype.hasOwnProperty() 方法'
date: 2019-10-08 14:42:13
categories: 
- Javascript
---


[参考链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)
Javascript 属性对象的 hasOwnProperty 方法 

**判断自身属性是否存在**
``` javascript
var o = new Object();
o.prop = 'exists';
console.log(o.hasOwnProperty('prop'));  // true

function changeO() {
  o.newprop = o.prop;
  delete o.prop;
}
changeO();

o.hasOwnProperty('prop');       // false
o.hasOwnProperty('newprop');    // true
```

**判断自身属性与继承属性**
``` javascript
function humam () {
    this.name = 'Jack'
    this.sayhello = function () {
        console.log('hello!')
    }
}
var boy = new human()
console.log(boy.hasOwnProperty('name'))         // true

boy.sayhello()      // hello!
console.log(boy.hasOwnProperty('sayhello'))     // true

// 继承
human.prototype.saygoodbye = function () {
  console.log('goodbye!')
}
boy.saygoodbye()    // goodbye!
console.log(boy.hasOwnProperty('saygoodbye'))   // false
console.log('saygoodbye' in boy)                // true
```

**使用遍历来忽略继承属性**
``` javascript
var orign = function () { this.x = 1 }
orign.prototype.y = 2

var o = new orign()
for (var name in o) {
    console.log(name, o.hasOwnProperty(name))
    // x true
    // y false
    // if (o.hasOwnProperty(name))
}
```

**注意 hasOwnProperty 作为属性名**
假如使用了 `hasOwnProperty` 作为属性名，则会获取不到真实结果
``` javascript
var o = {
    hasOwnProperty: function() {
        return false
    },
    foo: 'Hello, foo'
}
o.hasOwnProperty('foo') // 始终返回 false
```
只有使用 `另一个对象` 或者 `Object原型链` 上的 `hasOwnProperty` 属性，
可以避免上面的情况
``` javascript
// 匿名对象
({}).hasOwnProperty.call(o, 'foo')
// Object Prototype
Object.prototype.hasOwnProperty.call(o, 'foo')
```

