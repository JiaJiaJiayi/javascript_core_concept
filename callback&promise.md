## Callback
### Callback-based of synchronous programming
#### Error-first callback
```javascript
function A(callback){
    try{
    const data = await AnotherFunction();
    callback(data)}
    catch{
        callback(new Error("Something wrong"))
     }
}
A(function(error, data){
    if(error){
        // Handle error
    }
    else{
        // Handle data
    }
})
```

### Solve callback hell
Key: split several steps
```javascript
function A(callback){
    try{
    const data = await AnotherFunction();
    callback(data)}
    catch{
        callback(new Error("Something wrong"))
     }
}
step1(function(error, data){
    if(error){
        // Handle error
    }
    else{
        step2()
    }
})
step2(function(error, data){
    if(error){
        // Handle error
    }
    else{
        step3()
    }
})

A(step1);
```


## Promise
### Build chains of asynchronized action
```javascript
const r = new Promise(function (resolve, reject) {
  return setTimeout(function () {
    const data = 1;
    return resolve(data);
  }, 1000);
});

r.then((/*data*/data) => {
  console.log(data);
  // In then, we can create a new Promise
  return new Promise((resolve) =>
    setTimeout(() => {
      resolve(data * 2);
    }, 1000)
  );
}).then(/*function1*/(data2) => console.log(data2));
```

### Promise chain hell
有时候我们需要拿到之前`resolve`的结果，例如callback function: `function1` 需要拿到data,
则我们需要用到flattern的constructure
```javascript
new Promise(function (resolve, reject) {
  return setTimeout(function () {
    const data = 1;
    return resolve(data);
  }, 1000);
}).then((data) => {
  //   console.log(data);
  // In then, we can create a new Promise
  return new Promise((resolve) =>
    setTimeout(() => {
      resolve(data * 2);
    }, 1000)
  ).then(/*function1*/(data2) => console.log(data, data2)); //使用闭包的规则
});
```

### Custom handler return value
通常来说，then()handler中返回的未必是一个新的Promise，有几种情况：
1. return 一个新的promise
    ```javascript
    const a = new Promise(44);
    const b = a.then(()=> Promise.resolve(66));
    console.log(b); // Promise{<resolved>: 66}
    ```
    这种情况下，直接采用返回值。
2. return 其他值
    ```javascript
    const a = new Promise(44);
    const b = a.then(() => 66);
    console.log(b); // Promise{<resolved>: 66}
    ```
    javascript engine会把返回值wrap进一个promise.

#### Custom Thenbale Object
```javascript
class Thenable {
  constructor(num) {
    this.num = num;
  }
  then(resolve, reject) {
    // resolve with this.num*2 after the 1 second
    setTimeout(() => resolve(this.num * 2), 1000); // (**)
  }
}
```

### Implementation of Promise 
1. implementation code <https://github.com/then/promise/blob/master/src/core.js>
2. Promise A+ <https://github.com/promises-aplus/promises-spec#the-promise-resolution-procedure>
3. ECMA 262 <https://tc39.es/ecma262/#sec-promise-objects>
