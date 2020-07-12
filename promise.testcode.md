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

```javascript
// Let's get hold of those elements
var outer = document.querySelector('.outer');
var inner = document.querySelector('.inner');

// Let's listen for attribute changes on the
// outer element
new MutationObserver(function () {
  console.log('mutate');
}).observe(outer, {
  attributes: true,
});

// Here's a click listener…
function onClick() {
  console.log('click');

  setTimeout(function () {
    console.log('timeout');
  }, 0);

  Promise.resolve().then(function () {
    console.log('promise');
  });

  outer.setAttribute('data-random', Math.random());
}

// …which we'll attach to both elements
inner.addEventListener('click', onClick);
outer.addEventListener('click', onClick);
```
result: 

| Task1 | Task2 | Task3|
|-------|-------|------|
| Hanlder => microtask => Handler => microtask | PromiseHandler| PromiseHandler|
|click => promise => mutate => click => promise => mutate| promise | promise

