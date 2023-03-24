---
title: 021todos初版
categories: 百日博客计划
---

# 前言

日志，各位看官就当乐子看吧。

**正经人谁写日记啊？！！**    ——鹅城县长

# todos

项目初版基本好了，代码中保留了`axios`的代码，之后找找如何正确在`uniapp`中使用`axios`。

使用组件的一个难点就是很难操作其生成的dom对象。

有两个需求，没实现：
1. 当事项完成时，`uni-easyinput`生成的`input`能加上删除线，即`text-decoration:line-through;`
    方案是操作dom，但是只限于h5平台，微信小程序中，无法使用DOM，然后尝试用uniapp内置的获取节点方法`uni.createSelectorQuery()`，但取不到对应的input（h5下，是通过给组件绑定id，但是微信下，根本没有id属性）；
    ```javascript
    // 获取dom，添加删除线
    const input=document.querySelector('#todo-'+todo._id).querySelector("input.uni-input-input")
    if(input.getAttribute('style')==null) {
        input.setAttribute('style','text-decoration:line-through;')
    }else{
        input.removeAttribute('style')
    }
    ```
2. 数据初始化后，根据事项完成状态，添加删除线
    方案是操作dom, 在mounted中，获取生成的dom对象。结果发现没有获取到。
3. 使用`axios`发送请求
    实际中也有需要将vue项目改造成uniapp项目，除了改组件，JS代码都是希望仅可能复用的，找了第三方插件，如`uniapp-axios-adapter`，没实现。说一下使用过程，大家看看哪里不对：
    1. 安装`npm i axios@0.27.2`， `npm i uniapp-axios-adapter`
    2. `main.js`中配置全局API，按照原先vue项目的使用习惯
       1. 配置
            ```javascript
            // main.js        
            import request from "/common/http.js";
            // ...
            const app = new Vue({
            ...App
            })

            app.$mount()
            // axios 全局引用
            app.config.globalProperties.$axios = request;
            ```
            `http.js`文件
            ```javascript
            // /common/http.js
            import axios from "uniapp-axios-adapter";
            const request = axios.create({
            baseURL: "http://127.0.0.1:3000",
            timeout: 10000,
            });

            request.interceptors.request.use((config) => {
            //带上token
            // config.headers["Authorization"] = "token";
            return config;
            });

            request.interceptors.response.use((response) => {
            // 统一处理响应,返回Promise以便链式调用
            if (response.status === 200) {
                const { data } = response;
                if (data && data.code === 200) {
                return Promise.resolve(data);
                } else {
                return Promise.reject(data);
                }
            } else {
                return Promise.reject(response);
            }
            });

            export default request;
            ```
        2. 使用，直接调post或者按照文档方式都结果失败了
            ```javascript
            // 直接调
            axios.post('/task-modify', {
                id: this.todos[index]._id,
                title: editText
            }).then()
            // 文档案例的方式
            this.$axios({
                method: 'get',
                url: "/task-del",
                params: {
                    id: this.todos[delIndex]._id
                }
            }).then()
            ```

    3. 采取了直接使用的方式
        ```javascript
        // 官方文档的使用案例
        import axios from "uniapp-axios-adapter";
        axios.get({
            url: "https://example.com/api/getUserById",
            params: {
                id: 1,
            },
        });
        ```
        报错是，get is not defined，这就很疑惑了！

# 总结

三天，学习`uniapp`，使用其组件，简单实现一个todos项目

![todos真机演示](https://gitee.com/t0o-yang/Images/raw/master/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20230320223136.jpg)
