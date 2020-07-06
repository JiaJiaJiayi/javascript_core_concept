### EvalError
1. 不是现在Ecmascript规范中的，可自行抛出
```javascript
throw new EvalError('Hello', 'someFile.js', 10);
```

### RangeError
1. Passing not allowed string values yo String.prototype.normalize() 
   + `normalize()` method returns the Unicode Nomalization Form of the string
   For example:  sometimes more than one codepoint, or sequence of code points, can represent the same absract character
   
   ``` javascript
   let string1 = '\u00F1';
   let string2 = '\u006E\u0303';

   console.log(string1);  //  ñ
   console.log(string2);  //  ñ
   ```
2. Happens when attempting to create an array of illegal length wieh the Array constructor (larger than or equal to 2^32 || negative)
   Note: The max length value is pow(2,32) - 1, based on `ECMA-262 5th Edition`, when length of array is modified, runtime will use `ToUint32` to check.
   + mutable/ immmutable methods of Array.prototype
       - mutate: push/unshift, pop/shift, splice
       - immutate: concat, filter, map
3. Passing bad values to numeric methods
   + Number.prototype.toExponential(fractionDigits)  fractionDigits: 0-20
   + Number.prototype.toFixed(digits) digits: 0-100
   + Number.prototype.toPrecision(digits) digits: 0-100
   
### ReferenceError
1. Undefined variable

### SyntaxError
1. Syntactically invalid code.

### TypeError
Thrown when an operation can not be performed, typically (but not exclusively) when a value is not of the expected type
1. Operand or arguments passed to a function is incompatible with the type expected by that operator or function.
  ```javascript
    const a = {};
    a.UnknowFunction() => TypeError: a.UnknowFunction is not a function
  ```
2. When attempting to modify a value that cannot be changed.
  ```javascript
    const a = 12;
    a = 13; TypeError: Assignment to constant variable.
  ```
3. Use a value in appropriate way

### URIError

1. Why we should encode uri? [RFC URI: Generic Syntax](https://tools.ietf.org/html/rfc3986#:~:text=RFC%203986%20URI%20Generic%20Syntax%20January%202005%20representation%20is%20allowed,percent%2Dencoded%20for%20the%20URI.)
2. How we encode the characters? utf-8    
   1. ` "我".charCodeAt(0).toString(2); // "110001000010001"`
   2. use `codePoints => utf-8` Rule => it belongs to U+0800 ~ U+FFFF => 1110`001100` 10`001000` 10`010001`
   3. Hex: E68891 => `%E6%88%91`
3. Thrown when a global URI handling function was used in a wrong way.
  ```javascript
  decodeURIComponent('%')
  ```
  
### Custom Error
   
