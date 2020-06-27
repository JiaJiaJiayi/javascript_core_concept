### Call implementation
``` javascript
Function.prototype.call2 = function (context) {
  const object = context || window;
  object["fn"] = this;
  const args = [];
  // here arguments is an object like array, we can not use shift
  for (let i = 1; i < arguments.length; i++) {
    args[i - 1] = "arguments[" + i + "]";
  }
  // If we don't want to use es6 spread. const result = object.fn(...args);
  const result = eval("object.fn(" + args + ")");
  delete object["fn"];
  return result;
};

function b(name, age) {
  console.log("I am", this.a, name, age);
  return "result";
}

const result = b.call2({ a: 12 }, "haha", 26);
console.log(result);
```

### Apply implementation
``` javascript
Function.prototype.apply2 = function (context, arr) {
  const object = context || window;
  object["fn"] = this;

  for (i = 0; i < arr.length; i++) {
    args.push("arr[" + i + "]");
  }

  // If we don't want to use es6 spread. const result = object.fn(...args);
  const result = eval("object.fn(" + args + ")");
  delete object["fn"];
  return result;
};

function b(name, age) {
  console.log("I am", this.a, name, age);
  return "result";
}

const result = b.apply2({ a: 12 }, ["haha", 26]);
console.log(result);
```

### Bind implementation:
```javascript
const fn1 = fn.bind(obj);
fn1();
Function.prototype.bind2 = function (context) {
  const self = this;
  const predefinedArgs = Array.prototype.slice.call(arguments, 1);
  // const predefinedArgs = Array.prototype.slice.call(arguments,1);
  const f = function () {
    return self.apply(
      this instanceof f ? this : context,
      predefinedArgs.concat([...arguments])
    );
  };

  // normal bind function do not have properties in orginalFunction.prototype
  // But here we assign it to the to the oginalFunction's prototype to enable it.
  // b ----> b.prototype
  //            |
  //            |
  //            f (the new bound function)
  f.prototype = self.prototype;
  return f;
};
function b(name, age) {
  console.log(this.a);
  this.name = name;
  this.age = age;
  console.log(this.name, this.age);
  // return { result: "ok" };
}

const b1 = b.bind2({ a: 12 }, "jiajia");
console.log(b1.prototype);
console.log(b1(13));

function a() {
  console.log(this.c);
}
a.prototype.callA = "callA";

const a1 = a.bind({ c: 1 });
console.log(a1());
console.log(a1.callA);
```


### New implementation;
```javascript
function newFactory(constructorF) {
  const newObject = {};
  newObject.__proto__ = constructorF.prototype;
  const r = constructorF.call(newObject, [...arguments].slice(1));
  return typeof r === "object" ? r : newObject;
}

function a(name) {
  this.name = name;
}

const obj = newFactory(a, "jiajia");
console.log(obj instanceof a);
```
