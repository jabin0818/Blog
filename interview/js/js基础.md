

# js基础

## 闭包

- 最简单闭包示例：

  ```js
  function test() {
    const a = "111";
    function fun() {
      console.log(a);
    }
    return fun;
  }

  const test1 = test();
  test1();
  console.log("a");
  ```

  ​


###  如何知道promise的状态

```js
const PROMISE_STATE = {
  PENDING: 'pending',
  FULFILLED: 'fulfilled',
  REJECTED: 'rejected'
}
// 1、使用Promise.race
function decidePromiseState(promise) {
  const t = {};
  return Promise.race([promise, t])
  .then(v => (v === t) ? PROMISE_STATE.PENDING : PROMISE_STATE.FULFILLED)
  .catch(() => PROMISE_STATE.REJECTED)
}
// 2、使用宏任务与微任务
function decidePromiseState(promise) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(PROMISE_STATE.PENDING);
    }, 0);
    promise.then(() => {
      resolve(PROMISE_STATE.FULFILLED);
    }).catch(() => {
      resolve(PROMISE_STATE.REJECTED);
    });
  })
}
// 3、node环境
function decidePromiseState(promise) {
  const inspectStr = util.inspect(promise);
  if (inspectStr.includes(PROMISE_STATE.PENDING)) {
    return PROMISE_STATE.PENDING;
  } else if (inspectStr.includes(PROMISE_STATE.REJECTED)) {
    return PROMISE_STATE.REJECTED;
  } else {
    return PROMISE_STATE.FULFILLED;
  }
}
let a = Promise.resolve();
let b = Promise.reject();
let c = new Promise(() => {});

decidePromiseState(a).then(state => console.log(state)); // fulfilled

```

