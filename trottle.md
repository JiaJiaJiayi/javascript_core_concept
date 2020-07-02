### Debounce
```javascript
const debounceTime = 1000;

function handler(callback, wait) {
    let timeoutHandler = null;
    // 这里使用return一个function是方便首次传参
    // return /* outerFunction */function (evt) {
    return /* outerFunction */ function () {
        const args = arguments;
        if (timeoutHandler) clearTimeout(timeoutHandler);
        timeoutHandler = setTimeout(() => {
            // Here the arrow function do not have this, it will find the `this` of outer function.
            // callback.call(this, evt);
            callback.apply(this, args);
            timeoutHandler = null;
        }, wait);
    }
}

// ()=> {console.log(this)}; lexical scope => binds to global scope
// function(){ console.log(this)} later binding
document.getElementById("issue-233163700").addEventListener("mousedown", handler((function (evt) { console.log(evt, this) }), debounceTime));
```

### Debounce 
#### immediate版，立即执行，过n秒才可以再被触发
```javascript
const debounceTime = 10000;

function handler(callback, wait) {
    let timeoutHandler = null;
    const result = {
        f: function () {
            const args = arguments;
            const callNow = !timeoutHandler;
            if (callNow) {
                timeoutHandler = setTimeout(() => {
                    timeoutHandler = null;
                }, wait);
                return callback.apply(this, args);
            }
        },
        cancel: function () { timeoutHandler = null }
    }
    return result;
}
const r = handler(function (evt) { console.log(evt, this) }, debounceTime);

document.getElementById("issue-233163700").addEventListener("mousedown", r.f);
``` 

### cancel版debounce
```javascript
const debounceTime = 10000;

function handler(callback, wait) {
    let timeoutHandler = null;
    const f = function () {
        const args = arguments;
        const callNow = !timeoutHandler;
        if (callNow) {
            timeoutHandler = setTimeout(() => {
                timeoutHandler = null;
            }, wait);
            return callback.apply(this, args);
        }
    };
    f.cancel = function () { timeoutHandler = null }
    return f;
}
const r = handler(function (evt) { console.log(evt, this) }, debounceTime);

document.getElementById("issue-233163700").addEventListener("mousedown", r);
```

### throttle (time)
```javascript
// Use current - previous to compare the period, // It will trigger the first time 
function throttle(callback, period) {
    let previous = 0;
    return function () {
        const current = Date.now();
        if (current - previous > period) {
            callback.apply(this, arguments);
            previous = current;
        }
    };
}

document.getElementById("js-repo-pjax-container").addEventListener("mouseover", throttle(function (evt) { console.log(evt); }, 3000));
```
### throttle (setTimeout)
```javascript
// Use setTimeout, it will trigger when the timeout finished 
function throttle(callback, period) {
    let timeout = null;
    return function () {
        const args = arguments;
        if (!timeout) {
            timeout = setTimeout(() => {
                callback.apply(this, args);
                timeout = null;
            }, period);
        }
    };
}
```

### throttle (leading + trail)
```javascript
let num = 0;
document.getElementById("js-repo-pjax-container").addEventListener("mouseover", throttle(() => console.log(num++), 3000))

function throttle(callback, period) {
    let timeout;
    let previous = 0;
    return function () {
        const current = Date.now;
        const remaining = period - (current - previous);
        if (remaining <= 0) {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;

            }
            previous = current;
            callback.apply(this, arguments);
        } else if (!timeout) {
            setTimeout(() => {
                callback.apply(this, arguments);
                timeout = null;
                previous = current;
            }, remaining);
        }
    }
}
```
