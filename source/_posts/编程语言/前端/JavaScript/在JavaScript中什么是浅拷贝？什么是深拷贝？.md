---
categories:
  - 编程语言
  - 前端
  - JavaScript
---
### JavaScript中的数据类型

* 简单数据类型：直接存储在栈(stack)中的数据，例如String，Number类型等

* 引用数据类型：存储的是该对象**在栈中引用**，**真实的数据存放在堆内存里**，例如对象Object

而我们要讨论的浅拷贝与深拷贝是基于引用数据类型的；  

### 赋值与拷贝的区别

* 对于简单数据类型，赋值就是直接**新建一个栈中的内存**
* 而对于引用数据类型，赋值其实是赋的**该引用类型的内存地址**

而赋值与拷贝又有所不同，**拷贝**它会**创建一个新的对象**，这个对象是拷贝自原始的**对象属性**，并不是单纯的拷贝内存地址

### 浅拷贝与深拷贝

* **浅拷贝**： 

  * 拷贝的只是引用类型的原始对象的属性值，如果该原始对象的属性仍然是引用类型，那么只会拷贝内存地址，因此如果**拷贝的对象的引用类型属性值发生改变，是会影响另一个对象的**；
  * 浅拷贝只是拷贝一层，更深层次对象级别的只拷贝引用(地址)，所以改变新对象，旧对象也会改变，因为新旧对象共享一块内存。

* **深拷贝**： 深拷贝相比浅拷贝，即使原始对象的属性是引用类型，那么也会重新创建一个新的内存空间来拷贝原始属性值，即**修改拷贝对象的引用类型属性值也不会影响到另外一个对象的**；

### 实现浅拷贝的方式

* **Object.assign()**

```javascript
var obj={
	a:{
		name:1,
	},
	b:1
};
var obj1 = Object.assign({},obj);
obj1.a.name=2;
obj.b=2;
console.log(obj1,obj);
```

* **Array.prototype.concat（）**

```javascript
var obj = [
	  {
	     name: 1
	  }
]
var obj1 = obj.slice()
obj1[0].name = 2
console.log(obj[0].name) // 2
```

slice和splice最大的区别是，slice不会修改原数组，而splice会修改原数组

### 实现深拷贝的方式

* JSON.parse(JSON.stringify())

```javascript
var obj = [
    {
        name: 1,
    }
]
var obj1 = JSON.parse(JSON.stringify(obj))
obj1[0].name = 2
console.log(obj[0].name) // 1
```

但是JSON方法有它的缺点，如果包含时间对象，正则表达式，Error对象，函数，undefined，NaN，infinity等属性的时候，会丢失或者变成null；

**注意： 无法实现对象中方法的深拷贝。**

* 递归  
  当JSON方法不能满足我们深拷贝的需求时，我们应该手写一个递归函数

```javascript
function deepCopy(src) {
    if (typeof src !== 'object' || src === null) {
        return src
    }
    let dist = Array.isArray(src) ? [] : {}
    // 遍历引用类型
    for (let key in src) {
        let item = src[key]
        // 找到可迭代属性
        if (src.hasOwnProperty(key)) {
            if (item && typeof item === 'object') {
                // 还是引用类型则继续递归
                dist[key] = deepCopy(item)
            } else {
                // 简单数据类型直接复制
                dist[key] = item
            }
        }
    }
    return dist
}

var obj = {
    a: {
        name: 1
    }
}
var obj1 = deepCopy(obj)
obj1.a.name = 2
console.log(obj1.a.name, obj.a.name) // 2 1
```

* lodash的cloneDeep方法

```javascript
var _ = require('lodash');
var obj = {
   a: {
		name: 1
	},
   b: 1
};
var obj1 = _.cloneDeep(obj1);
obj1.a.name = 2
console.log(obj.a.name); // 1
```

*  jQuery 的 extend 方法

```javascript
$.extend([deep ], target, object1 [, objectN ])
```

* deep：表示是否深拷贝，true，为深拷贝；false，为浅拷贝。
* target：Object类型 目标对象，其他对象的成员属性将被附加到该对象上。
* object1： objectN 可选。 Object 类型 第一个以及第 N 个被合并的对象。

```javascript
let a = [0,1,[2,3],4]
let b = $.extend(true, [], a)
a[0] = 1
a[2][0] = 1  // [1,1,[1,3],4]
b  // [0,1,[2,3],4]
```
