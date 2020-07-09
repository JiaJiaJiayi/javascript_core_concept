### 一些关于Promise的题目
```javascript
const promise1 = new Promise((r) => {
  r(
    new Promise((r) =>
      r(
        new Promise((r) => {
          r(Promise.resolve(4));
          console.log(4);
        })
      )
    )
  );
}).then(x => console.log(3));
console.log(1); // output: 4 => 1 => 3;
```
