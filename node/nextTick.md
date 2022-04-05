```javascript
function getTicker(log) {
  log = log || Function.prototype;

  var nativeNextTick = process.nextTick;
  var ticking = false;

  function nextTick() {
    if (!ticking) {
      ++nextTick._tick;
      ticking = true;
      nativeNextTick(function () {
        log(nextTick._tick);
        // Do not put this statement before the one above
        // That would cause `nextTick` calling itself forever
        ticking = false;
      });
    }

    nativeNextTick.apply(null, arguments);
  }

  nextTick._tick = 0;

  return nextTick;
}

const polyfill = function (opts) {
  // already hacked

  if (process.nextTick._tick != null) return;

  var env = process.env.NODE_DEBUG;
  if (!env || !/\bnexttick\b/.test(env)) return;

  opts = opts || {};

  var template = '---------- TICK %d ----------';
  if (opts.color) {
    template = '\x1b[32m' + template + '\x1b[0m';
  }

  var log =
    opts.log !== false &&
    function (tick) {
      console.error(template, tick);
    };

  process.nextTick = getTicker(log);
};

polyfill({ color: true });

// var n = 3;

// var rs = require('stream').Readable({
//   read: function () {
//     process.nextTick(function (prefix) {
//       if (n) {
//         rs.push(prefix + n-- + '\n');
//       } else {
//         rs.push(null);
//       }
//     }, 'DATA: ');
//   },
// });
// rs.pipe(process.stdout);

console.log('before');
const p = Promise.resolve(1).then((res) => console.log(res));
console.log('after');

```
