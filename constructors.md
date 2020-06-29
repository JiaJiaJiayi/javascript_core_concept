### 工厂模式
```javascript
function createObj(name) {
    const obj = {
        name,
        getName: function () { return this.name }
    };
    // obj.name = name;
    // obj.getName = function(){ return name};
    return obj;
}

const object = createObj("chaochao");
console.log(object.getName());

//构造函数模式
//每个实例创建一次getName，浪费空间
function Person(name) {
    this.name = name;
    this.getName = function () {
        return this.name;
    }
}

const person = new Person("jiajia");
console.log(person.getName());
```

### 构造函数模式优化
```javascript
// cons: 无封装？？
function getName() {
    return this.name;
}
function Person2(name) {
    this.name = name;
    this.getName = getName;
}

const person2 = new Person2("jia&chao");
console.log(person2.getName());
```

### 原型模式
```javascript
function Person3(name) {
    this.name = name;
}
Person3.prototype.getName = function () { return this.name };
// 也可以写成
// Person3.prototype ={
//     constructor: Person3,
//     getName =  function () { return this.name }
// }
const p3 = new Person3("yang ma");
console.log(p3.getName());
```

### 动态原型模式
```javascript
// function Person4(name) {
//     this.name = name;
// // 未必是 Person4.prototype.getName， 只要原型链上有就可以了
//     if (Person4.prototype.getName) {
//         Person4.prototype = {
//             constructor: Person4,
//             getName: function () { return this.name }
//         }
//     }
// }

// const p4 = new Person4("p4");
// // 在执行构造函数Person4之前，
// // 将p4.__proto__指向Person4.prototype，之后再重新指定，就失去链接
// //
// console.log(p4.getName()) // getName is not a function

function Person4(name) {
    this.name = name;
    if (typeof this.getName !== "function") {
        Person4.prototype.getName =
            function () { return this.name }
    }
}

const p4 = new Person4("p4");
// 在执行构造函数Person4之前，
// 将p4.__proto__指向Person4.prototype，之后再重新指定，就失去链接
// 
console.log(p4.getName())
```

### 寄生构造函数模式
```javascript
// 令人困惑，好像和工厂模式差不多？
function Person5(name) {
    const obj = {
        name,
        getName: function () { return this.name }
    }
    return obj;
}
// 使用new初始化
const p5 = new Person5("p5");
// 使用场景： 构建一个需要额外方法的数组， 不影响原来的使用

// little tips:
// 把数组的属性放到arguments上可以这么使用
// 原因是aguments是array-like object!!
// arr.push.apply(arr, arguments);
```

### 稳妥构造函数
```javascript
// 使用闭包，没有公共属性，则只能在初始化设置属性
function Person6(name, age) {
    // 没有return this, 所以instanceof判定是有问题的
    return {
        getName: () => name,
        getAge: () => age
    }
}

const p6 = new Person6("chaochaoba", 50);
console.log(p6.getName(), p6.getAge());
console.log(p6 instanceof Person6)
```
