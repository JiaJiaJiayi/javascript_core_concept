### 使用iterator protocal实现的
```javascript
const iterator = function () {
    let i = 0;
    return {
        next: () => ({
            value: i++,
            done: i === 100
        })
    }
}
const obj = {};
obj[Symbol.iterator] = iterator;
console.log([...obj]);
```
### 使用generator
```javascript
function* generator(start = 0) {
    let i = start;
    while (i < 100) {
        yield i++;
    }
}

const i = generator(0);
// while (!i.done) {
console.log(i.next().value);
console.log(i.next().value);
// }


// const i = obj[Symbol.iterator]();
// console.log(i.next());
// console.log(i.next());
```

### 使用generator循环到结束
```javascript
function* generator1() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
}
const gen = generator1();
let next = gen.next();
while (!next.done) {
  console.log(next.value);
  next = gen.next();
}

```
