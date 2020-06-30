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
