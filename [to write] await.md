### Await and microtask 

1. Code 1
  ```javascript
  function three() {
    console.log("three");
  }
  function two() {
    console.log("two");
  }

  async function one() {
    console.log("one");
    await two();
    three();
  }

  one();
  console.log("end"); 
  ```
  output: one => two => end => three


2. Code 2
  ```javascript
  function three() {
    console.log("three");
  }
  function two() {
    console.log("two");
  }

  async function one() {
    console.log("one");
    await two();
    three();
  }

  await one(); // Because `await` here, the whole rest of script will run in microtask.
  console.log("end");
  ```
  output: one => two => three => end
