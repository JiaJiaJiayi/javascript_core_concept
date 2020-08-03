```javascript

function shuffle(arr) {
  for (let i = arr.length - 1; i; i--) {
    const random = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[random]] = [arr[random], arr[i]];
  }
  return arr;
}

function oppositeShuffle(arr) {
  const n = arr.length;
  for (let i = 0; i < n - 1; i++) {
    const random = Math.floor(Math.random() * (n - i)) + i;
    [arr[i], arr[random]] = [arr[random], arr[i]];
  }
  return arr;
}

// create by self, in reverse order
function shuffle2(arr) {
  const length = arr.length;
  for (let i = 1; i < length; i++) {
    const random = Math.floor(Math.random() * (i + 1)); // select a random number from [0, i];
    [arr[i], arr[random]] = [arr[random], arr[i]];
  }
  return arr;
}
const arr = [...Array(11).keys()];

const times = 10000;

function countTest(arr, fn) {
  const parameters = [...arguments].slice(2);
  const elementNumber = arr.length;
  let sum = Array(elementNumber)
    .fill(0)
    .map((v) => Array(elementNumber).fill(v));
  let startTime = Date.now();
  for (let i = 0; i < times; i++) {
    let output = fn.call(null, [...arr], ...parameters);
    output.forEach((e, ind) => sum[e][ind]++);
  }
  console.log(Date.now() - startTime);
  console.table(
    sum.map((row) => row.map((cell) => ((cell * 100) / times).toFixed(2) + "%"))
  );
}
// countTest(arr, shuffle);
// countTest(arr, oppositeShuffle);
// countTest(arr, shuffleBySortRandomOrder);

// 如果用快排，是不是就很random了？
function shuffleBySort(arr) {
  return arr.sort(() => Math.random() - 0.5);
}
// countTest(arr, shuffleBySortRandomOrder);

// 为什么sort用时这么久？
function shuffleBySortRandomOrder(arr) {
  var random = arr.map(Math.random);
  return arr.sort((a, b) => random[a] - random[b]);
}

let cb = () => Math.random() - 0.5;
function quickSort(arr, left, right) {
  var i = left;
  var j = right;
  var tmp;
  var pivotidx = (left + right) / 2;
  var pivot = parseInt(arr[pivotidx.toFixed()]);
  /* partition */
  while (i <= j) {
    while (cb(parseInt(arr[i]), pivot) < 0) i++;
    while (cb(parseInt(arr[j]), pivot) > 0) j--;
    if (i <= j) {
      tmp = arr[i];
      arr[i] = arr[j];
      arr[j] = tmp;
      i++;
      j--;
    }
  }

  /* recursion */
  if (left < j) quickSort(arr, left, j);
  if (i < right) quickSort(arr, i, right);
  return arr;
}
var array = [...Array(10).keys()];
// quickSort(array, 0, array.length - 1);

countTest(array, quickSort, 0, array.length - 1);

```
