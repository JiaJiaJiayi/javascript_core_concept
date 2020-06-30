### 原型链继承
```javascript
// pros: instanceof 判定正确
// cons: 1. 不能指定parent中属性，如果更改属性后则被所有child的instance共享
function Parent1() {
    this.name = 'jiajia';
}
Parent1.prototype.getName = function () { return this.name };

function Child1() {
    this.age = 13;
}
Child1.prototype = new Parent1();
console.log("c1", new Child1().name);
console.log("c1", new Child1() instanceof Parent1)
```

### 借用构造函数（经典继承）
```javascript
// pros: 借用parent的构造函数，实现给parent指定参数
// cons: parent constructor不在原型链上，所以方法需要在Parent2的构造函数中定义，每次新生成child实例都会生成方法
function Parent2(name) {
    this.name = name;
}

function Child2(name, age) {
    Parent2.call(this, name);
    this.age = age;
}

console.log("c2", new Child2("chaochao", 18).name);
console.log("c2", new Child2("jiajia", 17) instanceof Parent2);
```

### 组合继承
```javascript
// 通过借用构造函数，解决parent属性共享问题，parent中属性实际定义在child中，
// 通过改变prototype指向，解决instanceof 问题
// cons: 多次调用Parent3构造函数
function Parent3(name) {
    this.name = name;
}

function Child3(name, age) {
    this.age = age;
    // 通过借用构造函数，可不共享parent中属性
    // 属性实际存在于child上
    Parent3.call(this, name);
}

Parent3.prototype.getInfo = function () { return this.name + " " + this.age };

Child3.prototype = new Parent3();
Child3.prototype.constructor = Child3;

const i3 = new Child3("chaoba", 50);
console.log("c3", i3.getInfo());
const i4 = new Child3("chaoma", 49);
console.log("c3", i3 instanceof Child3, i3 instanceof Parent3);
```

### 原型式继承(类似于工厂方法）
```javascript
// cons: 所有child对象共享同一个parentObj，无法指定parent中属性
function Parent4(name) {
    this.name = name;
}

function createObj(parentObj, age) {
    const obj = Object.create(parentObj);
    parentObj.__proto__.getInfo = function () { return this.name + ' ' + this.age };
    obj.age = age;
    return obj;
}
const parentObj = new Parent4("p4");
const c4 = createObj(parentObj, 20);
console.log(c4.getInfo());
console.log(c4 instanceof Parent4);
```

### 寄生组合式继承
```javascript
function Parent5(name) {
    this.name = name;
}

function Child5(name, age) {
    this.age = age;
    Parent5.call(this, name);
}
Parent5.prototype.getInfo = function () { return this.name + ' ' + this.age }

// // 问题：与下列代码是否等效？
// // Child5.prototype.__proto__ = Parent5.prototype;
// function f() { }
// f.prototype = Parent5.prototype;
// Child5.prototype = new f();
// Child5.prototype.constructor = Child5;

// const c5 = new Child5("c5", 20);
// console.log(c5.getInfo());
// console.log(c5 instanceof Child5, c5.__proto__.constructor === Child5, c5 instanceof Parent5);
// const c52 = new Child5("C52", 30);
// console.log(c5.getInfo())

// 封装
function getPrototype(parent) {
    function a() { }
    a.prototype = parent.prototype;
    return new a();
}

function createInherence(childConstructor, parentConstructor) {
    const p = getPrototype(parentConstructor);
    childConstructor.prototype = p;
    p.constructor = childConstructor;
}

createInherence(Child5, Parent5);
const c6 = new Child5("c6", 0);
console.log(c6 instanceof Child5, c6 instanceof Parent5, c6.getInfo())
```
