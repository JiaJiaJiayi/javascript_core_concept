#### 转载自 stackoverflow

A little bit of background:

ECMAScript 6+ distinguishes between *callable* (can be called without `new`) and *constructable* (can be called with `new`) functions:

- Functions created via the arrow functions syntax or via a method definition in classes or object literals are *not constructable*.
- Functions created via the `class` syntax are *not callable*.
- Functions created in any other way (function expression/declaration, `Function` constructor) are callable and constructable.
- Built-in functions are not constructrable unless explicitly stated otherwise.

**About `Function.prototype`**

`Function.prototype` is a so called [*built-in function*][1] [that is not constructable][2]. From the spec:

> Built-in function objects that are not identified as constructors do not implement the `[[Construct]]` internal method unless otherwise specified in the description of a particular function. 

The value of `Function.prototype` is create at the very beginning of the runtime initialization. It is basically an empty function and it is not explicitly stated that it is constructable.

---

> How do I check if an function is a constructor so that it can be called with a new? 

There isn't a built-in way to do that. You can `try` to call the function with `new`, and either inspect the error or return `true`:

    function isConstructor(f) {
      try {
        new f();
      } catch (err) {
        // verify err is the expected error and then
        return false;
      }
      return true;
    }

However, that approach is not failsafe since functions can have side effects, so after calling `f`, you don't know which state the environment is in.

Also, this will only tell you whether a function *can* be called as a constructor, not if it is *intended* to be called as constructor. For that you have to look at the documentation or the implementation of the function.

**Note:** There should never be a reason to use a test like this one in a production environment. Whether or not a function is supposed to be called with `new` should be discernable from its documentation. 

> When I create a function, how do I make it NOT a constructor?

To create a function is truly not *constructable*, you can use an arrow function:

    var f = () => console.log('no constructable');

Arrow functions are by definition not constructable. Alternatively you could define a function as a method of an object or a class.

Otherwise you could check whether a function is called with `new` (or something similar) by checking it's `this` value and throw an error if it is:

    function foo() {
      if (this instanceof foo) {
        throw new Error("Don't call 'foo' with new");
      }
    }

Of course, since there are other ways to set the value of `this`, there can be false positives.

---

**Examples**

<!-- begin snippet: js hide: false console: true babel: false -->

<!-- language: lang-js -->

    function isConstructor(f) {
      try {
        new f();
      } catch (err) {
        if (err.message.indexOf('is not a constructor') >= 0) {
          return false;
        }
      }
      return true;
    }

    function test(f, name) {
      console.log(`${name} is constructable: ${isConstructor(f)}`);
    }

    function foo(){}
    test(foo, 'function declaration');
    test(function(){}, 'function expression');
    test(()=>{}, 'arrow function');

    class Foo {}
    test(Foo, 'class declaration');
    test(class {}, 'class expression');

    test({foo(){}}.foo, 'object method');

    class Foo2 {
      static bar() {}
      bar() {}
    }
    test(Foo2.bar, 'static class method');
    test(new Foo2().bar, 'class method');

    test(new Function(), 'new Function()');

<!-- end snippet -->

  [1]: http://www.ecma-international.org/ecma-262/7.0/#sec-built-in-function
  [2]: http://www.ecma-international.org/ecma-262/7.0/#sec-createintrinsics
