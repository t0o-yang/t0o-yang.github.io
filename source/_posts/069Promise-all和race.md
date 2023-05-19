---
title: 069Promise-all和race
tags: []
categories:
  - 百日博客计划
date: 2023-05-19 22:41:37
description: 手写Promise all和race的逻辑
---

#### Promise all和race逻辑

```js
MyPromise.resolve = function (value) {
  return new MyPromise((resolve, reject) => resolve(value));
};
MyPromise.reject = function (reason) {
  return new MyPromise((resolve, reject) => reject(reason));
};

MyPromise.all = function (promiseList = []) {
  const p1 = new MyPromise((resolve, reject) => {
    const result = [];
    const length = promiseList.length;
    let resolveCount = 0;

    promiseList.forEach((p) => {
      p.then((data) => {
        result.push(data);
        resolveCount++;
        if (resolveCount === length) {
          resolve(result);
        }
      }).catch((err) => reject(err));
    });
  });
  return p1;
};

MyPromise.race = function (promiseList = []) {
  let resolved = false;
  const p1 = new MyPromise((resolve, reject) => {
    promiseList.forEach((p) => {
      p.then((result) => {
        if (!resolved) {
          resolve(result);
          resolved = true;
        }
      }).catch((err) => {
        reject(err);
      });
    });
  });
  return p1;
};
```