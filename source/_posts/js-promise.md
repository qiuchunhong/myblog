---
title: js-promise
categories: js
tags: promise
summary:
---

## promise 执行顺序

- resolve() 后面的也是同步任务，跟 resolve 前面的同步执行。
- return resolve() 后面的内容不执行。

```javascript
let promise = new Promise(function (resolve, reject) {
  console.log("Promise");
  resolve();
  console.log("Promise2");
});
promise.then(function () {
  console.log("resolved");
});
console.log("Hi!");
// Promise
// Hi!
// resolved
```

## reject()后执行

> 一般总是建议，Promise 对象后面要跟 catch()方法，这样可以处理 Promise 内部发生的错误。catch()方法返回的还是一个 Promise 对象，因此后面还可以接着调用 then()方法

```javascript
let promise = new Promise(function (resolve, reject) {
  console.log("Promise");
  reject();
});
promise
  .then(function () {
    console.log("resolved");
  })
  .catch(() => {
    console.log("err");
  });
// Promise
// err
promise
  .catch(() => {
    console.log("err");
  })
  .then(function () {
    console.log("resolved");
  });
// Promise
// err
// resolved
```

## resolve 函数和 reject 函数时带有参数

如果调用 resolve 函数和 reject 函数时带有参数，那么它们的参数会被传递给回调函数。reject 函数的参数通常是 Error 对象的实例，表示抛出的错误；resolve 函数的参数除了正常的值以外，还可能是另一个 Promise 实例，比如像下面这样。

```javascript
const p1 = new Promise(function (resolve, reject) {
  setTimeout(() => {
    console.log("2");
    reject(new Error("fail"));
    console.log(3);
  }, 2000);
});
const p2 = new Promise(function (resolve, reject) {
  setTimeout(() => {
    console.log("1");
    resolve(p1);
    console.log(4);
  }, 4000);
});
p2.then((result) => console.log(result, "r")).catch((error) =>
  console.log(error, "e")
);
// 2 3 1 4 Error: fail "e"
```

这时 p1 的状态就会传递给 p2，也就是说，p1 的状态决定了 p2 的状态。如果 p1 的状态是 pending，那么 p2 的回调函数就会等待 p1 的状态改变；如果 p1 的状态已经是 resolved 或者 rejected，那么 p2 的回调函数将会立刻执行

## Promise.prototype.finally()

> finally()方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。

```javascript
promise
  .then((result) => {
    console.log(1);
  })
  .catch((error) => {
    console.log(2);
  })
  .finally(() => {
    console.log(3);
  });
```

上面代码中，不管 promise 最后的状态，在执行完 then 或 catch 指定的回调函数以后，都会执行 finally 方法指定的回调函数

## Promise.all()

> 如果作为参数的 Promise 实例，自己定义了 catch 方法，那么它一旦被 rejected，并不会触发 Promise.all()的 catch 方法。
> const p = Promise.all([p1, p2, p3]);

**all**

- 只有 p1、p2、p3 的状态都变成 fulfilled，p 的状态才会变成 fulfilled，此时 p1、p2、p3 的返回值组成一个数组，传递给 p 的回调函数。
- 只要 p1、p2、p3 之中有一个被 rejected，p 的状态就变成 rejected，此时第一个被 reject 的实例的返回值，会传递给 p 的回调函数。

```
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result)
.catch(e => e);
const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result)
.catch(e => e);
Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// ["hello", Error: 报错了]
```

上面代码中，p1 会 resolved，p2 首先会 rejected，但是 p2 有自己的 catch 方法，该方法返回的是一个新的 Promise 实例，p2 指向的实际上是这个实例。该实例执行完 catch 方法后，也会变成 resolved，导致 Promise.all()方法参数里面的两个实例都会 resolved，因此会调用 then 方法指定的回调函数，而不会调用 catch 方法指定的回调函数。

如果 p2 没有自己的 catch 方法，就会调用 Promise.all()的 catch 方法。

## Promise.race()

const p = Promise.race([p1, p2, p3]);
**race**

- 只要 p1、p2、p3 之中有一个实例率先改变状态，p 的状态就跟着改变。
- 那个率先改变的 Promise 实例的返回值，就传递给 p 的回调函数

## Promise.allSettled()

> Promise.allSettled()方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例。只有等到所有这些参数实例都返回结果，不管是 fulfilled 还是 rejected，包装实例才会结束。该方法由 ES2020 引入。

> 该方法返回的新的 Promise 实例，一旦结束，状态总是 fulfilled，不会变成 rejected。状态变成 fulfilled 后，Promise 的监听函数接收到的参数是一个数组，每个成员对应一个传入 Promise.allSettled()的 Promise 实例。

```javascript
const resolved = Promise.resolve(42);
const rejected = Promise.reject(-1);
const allSettledPromise = Promise.allSettled([resolved, rejected]);
allSettledPromise.then(function (results) {
  console.log(results);
});
// [
//    { status: 'fulfilled', value: 42 },
//    { status: 'rejected', reason: -1 }
// ]
```

有时候，我们不关心异步操作的结果，只关心这些操作有没有结束
下面是返回值用法的例子。

```javascript
const promises = [fetch("index.html"), fetch("https://does-not-exist/")];
const results = await Promise.allSettled(promises);
// 过滤出成功的请求
const successfulPromises = results.filter((p) => p.status === "fulfilled");
// 过滤出失败的请求，并输出原因
const errors = results
  .filter((p) => p.status === "rejected")
  .map((p) => p.reason);
```

## Promise.any()

> ES2021 引入了 Promise.any()方法。该方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例返回。只要参数实例有一个变成 fulfilled 状态，包装实例就会变成 fulfilled 状态；如果所有参数实例都变成 rejected 状态，包装实例就会变成 rejected 状态。

## 实现 Promise 完整代码

```javascript
class MyPromise {
  constructor(fn) {
    this.fulfilledQueue = [];
    this.rejectedQueue = [];
    this._status = PENDING;
    this._value = null;

    const handleFulfilledQueue = () => {
      while (this.fulfilledQueue.length) {
        let fulfiledFn = this.fulfilledQueue.shift();
        fulfiledFn(this._value);
      }
    };
    const handleRejectedQueue = () => {
      console.log(this.rejectedQueue);
      while (this.rejectedQueue.length) {
        let rejectedFn = this.rejectedQueue.shift();
        rejectedFn(this._value);
      }
    };

    // 完成状态转变，执行回调队列中的回调函数
    const _resolve = (val) => {
      const fn = () => {
        if (this._status !== PENDING) {
          return;
        }
        if (val instanceof MyPromise) {
          val.then(
            (res) => {
              this._status = FULFILLED;
              this._value = res;
              handleFulfilledQueue();
            },
            (err) => {
              this._status = REJECTED;
              this._value = err;
              handleRejectedQueue();
            }
          );
        } else {
          this._status = FULFILLED;
          this._value = val;
          handleFulfilledQueue();
        }
      };
      setTimeout(fn, 0);
    };
    // 完成状态Pending到REJECTED的转变，执行rejected队列中的回调函数
    const _reject = (val) => {
      const fn = () => {
        if (this._status !== PENDING) {
          return;
        }
        this._status = REJECTED;
        this._value = val;
        handleRejectedQueue();
      };
      setTimeout(fn, 0);
    };

    try {
      // 处理外部传入函数执行异常
      fn(_resolve, _reject);
    } catch (e) {
      return _reject(e);
    }
  }

  then(successFn, failFn) {
    return new MyPromise((resolve, reject) => {
      // 执行成功时的回调函数
      const handleSucess = (fn) => {
        try {
          if (typeof fn === "function") {
            const res = fn(this._value);
            if (res instanceof MyPromise) {
              res.then(resolve, reject);
            } else {
              resolve(res);
            }
          } else {
            resolve(this._value);
          }
        } catch (e) {
          reject(e);
        }
      };
      // 执行失败时的回调函数
      const handleFail = (fn) => {
        try {
          if (typeof fn === "function") {
            const res = fn(this._value);
            if (res instanceof MyPromise) {
              res.then(resolve, reject);
            } else {
              resolve(res);
            }
          } else {
            reject(this._value);
          }
        } catch (e) {
          reject(e);
        }
      };
      switch (this._status) {
        case PENDING: // 异步任务尚未完成，将回调函数推入相应队列
          this.fulfilledQueue.push(() => {
            handleSucess(successFn);
          });
          this.rejectedQueue.push(() => {
            handleFail(failFn);
          });
          break;
        case FULFILLED: // 异步任务成功完成，执行成功回调函数
          handleSucess(successFn);
          break;
        case REJECTED: // 异步任务已失败，执行失败回调函数
          handleFail(failFn);
          break;
        default:
          console.log("Promise error status:", this._status);
          break;
      }
    });
  }

  catch(failFn) {
    return this.then(null, failFn);
  }

  finally(finalFn) {
    return this.then(finalFn, finalFn);
  }

  static resolve(val) {
    if (val instanceof MyPromise) {
      return val;
    } else {
      return new MyPromise((resolve, reject) => {
        resolve(val);
      });
    }
  }

  static reject(val) {
    return new MyPromise((resolve, reject) => {
      reject(val);
    });
  }

  static all(promiseArr) {
    return new Promise((resolve, reject) => {
      const len = promiseArr.length;
      let count = 0;
      let result = [];
      for (let i = 0; i < len; i++) {
        promiseArr[i].then(
          (val) => {
            count++;
            result.push[val];
            if (count === len) {
              resolve(result);
            }
          },
          (err) => {
            reject(err);
          }
        );
      }
    });
  }

  static race(promiseArr) {
    return new Promise((resolve, reject) => {
      const len = promiseArr.length;
      for (let i = 0; i < len; i++) {
        promiseArr[i].then(
          (val) => {
            resolve(val);
          },
          (err) => {
            reject(err);
          }
        );
      }
    });
  }

  static allSettled(allPromise) {
    return new Promise((resolve) => {
      let settled = 0;
      let result = [];
      let len = allPromise.length;
      for (let i = 0; i < len; i++) {
        allPromise[i]
          .then((res) => {
            result[index] = { status: "fulfilled", value: res };
          })
          .catch((err) => {
            result[index] = { status: "rejected", reason: err };
          })
          .finally(() => {
            ++settled === len && resolve(result);
          });
      }
    });
  }
}
```
