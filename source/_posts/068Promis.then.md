---
title: 068Promise.then
tags: []
categories:
  - 百日博客计划
date: 2023-05-18 21:59:07
description: 手写Promise.then
---

#### Promise.then

```js
then(fn1, fn2) {
  fn1 = typeof fn1 === "function" ? fn1 : (v) => v;
  fn2 = typeof fn2 === "function" ? fn2 : (e) => e;
  if (this.state === "pending") {
    const p1 = new MyPromise((resolve, reject) => {
      this.resolveCallbacks.push(() => {
        try {
          const newResult = fn1(this.result);
          resolve(newResult);
        } catch (err) {
          reject(err);
        }
      });
      this.rejectCallbacks.push(() => {
        try {
          const newReason = fn2(this.reason);
          reject(newReason);
        } catch (err) {
          reject(err);
        }
      });
    });
    return p1;
  }
  if (this.state === "fulfilled") {
    const p1 = new MyPromise((resolve, reject) => {
      try {
        const newResult = fn1(this.result);
        resolve(newResult);
      } catch (err) {
        reject(err);
      }
    });
    return p1;
  }
  if (this.state === "rejected") {
    const p1 = new MyPromise((resolve, reject) => {
      try {
        const newReason = fn2(this.reason);
        reject(newReason);
      } catch (err) {
        reject(err);
      }
    });
    return p1;
  }
}
catch(fn2) {
  return this.then(null, fn2);
}
```