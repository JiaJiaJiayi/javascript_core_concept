// Use current - previous to compare the period,
// It will trigger the first time
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

document.getElementById("js-repo-pjax-container").addEventListener(
  "mouseover",
  throttle(function (evt) {
    console.log(evt);
  }, 3000)
);

// Use setTimeout,  it will trigger when the timeout finished
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

//有头有尾

