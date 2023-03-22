---
title: 007实现Modal
---

# 前言

日志，各位看官就当乐子看吧。

**正经人谁写日记啊？！！**    ——鹅城县长

不用想乱七八糟的，写就完事儿了。

# Vue3

## 实现 Modal

不用看我的文章了（感觉我只能背这段代码了），直接看原文去理解吧，链接：https://segmentfault.com/a/1190000038928664

实现一个`Modal`模态对话框，首先分析一下结构和能够预想到的内容，需要的结构：
- 遮罩层
- 标题
- 主体内容
- 按钮

主体部分可能是一段文字，也可能是段`HTML`代码。好了，下面具体实现。

### 目录结构

`Modal`会被`app.use(Modal)`作为一个插件调用，所以放在`plugin`文件夹中。
```shell
├── plugins
│   └── modal
│       ├── Content.tsx // 维护 Modal 的内容，用于 h 函数和 jsx 语法
│       ├── Modal.vue // 基础组件
│       ├── config.ts // 全局默认配置
│       ├── index.ts // 入口
│       ├── locale // 国际化相关
│       │   ├── index.ts
│       │   └── lang
│       │       ├── en-US.ts
│       │       ├── zh-CN.ts
│       │       └── zh-TW.ts
│       └── modal.type.ts // ts类型声明相关
```

### 组件代码

对话框通常存在于vue实例之外，挂载在`body`上。
```html
// Modal.vue
<template>
    <Teleport to="body" :disabled="!isTeleport">
        <div v-if="modelValue" class="modal">
            <!-- 遮罩层 -->
            <div
                class="mask"
                :style="style"
                @click="maskClose && !loading && handleCancel()"
                >
            </div>
            <div class="modal__main">
                <!-- 标题区域 -->
                <div class="modal__title line line--b">
                    <span>{{ title || t("r.title") }}</span>
                    <span
                        v-if="close"
                        :title="t('r.close')"
                        class="close"
                        @click="!loading && handleCancel()"
                        >✕</span
                        >
                </div>
                <!-- 主体内容区域 -->
                <div class="modal__content">
                    <Content v-if="typeof content === 'function'" :render="content" />
                    <slot v-else>
                        {{ content }}
                    </slot>
                </div>
                <!-- 按钮 -->
                <div class="modal__btns line line--t">
                    <button :disabled="loading" @click="handleConfirm">
                        <span class="loading" v-if="loading"> ❍ </span>{{ t("r.confirm") }}
                    </button>
                    <button @click="!loading && handleCancel()">
                        {{ t("r.cancel") }}
                    </button>
                </div>
            </div>
        </div>
    </Teleport>
</template>
```
使用`teleport`将内容传送到`body`标签下。关注主体内容代码，
```html
<div class="modal__content">
    <Content v-if="typeof content === 'function'" :render="content" />
    <slot v-else>
        {{ content }}
    </slot>
</div>
```
其中`Content`组件是为了兼容`Modal`以API形式调用的方式。`Content`：
```javascript
// Content.tsx
import { h } from 'vue';
const Content = (props: { render: (h: any) => void }) => props.render(h);
Content.props = ['render'];
export default Content;
```
当基于API调用`Modal`，如，
- h 渲染函数
```javascript
//  h 渲染函数
$modal.show({
    title: '演示 h 函数',
    content(h){
        return h(
            'div',
            {
                style: 'color:red;',
                onClick: ($event)=>{ console.log('click', $event.target)}
            },
            'hello world'
        )
    }
})
```
- JSX 语法
```JSX
// JSX（javascript的扩展，标记语言和js可以一起写）
$modal.show({
  title: '演示 jsx 语法',
  content() {
    return (
      <div
        onClick={($event: Event) => console.log('clicked', $event.target)}
      >
        hello world ~
      </div>
    );
  }
});
```
另一种方式（v-else分支内），对主体内容的填写通过默认插槽或者prop

```html
<Modal v-model='show' title='默认插槽'>
    <div>hello world</div>
</Modal>
// content 属性
<Modal v-model="show" title="演示 content" content="hello world~" />
```

如上，支持通过`4种方式`使用`Modal`组件。

### 组价API化

`Vue3`中实现组件API化，可以这么写，
```javascript
import Modal from './Modal.vue';
const container = document.createElement('div');
const vnode = createVNode(Modal);
render(vnode, container);
const instance = vnode.component;
document.body.appendChild(container);
```
将组件转化成虚拟`dom`，再通过渲染函数渲染到div上，最后动态添加到body中。
看具体代码，
```javascript
// index.ts
import { App, createVNode, render } from 'vue';
import Modal from './Modal.vue';
import config from './config';

// 新增 Modal 的 install 方法，为了可以被 `app.use(Modal)`（Vue使用插件的的规则）
Modal.install = (app: App, options) => {
  // 可覆盖默认的全局配置
  Object.assign(config.props, options.props || {});

  // 注册全局组件 Modal
  app.component(Modal.name, Modal);
  
  // 注册全局 API
  app.config.globalProperties.$modal = {
    show({
      title = '',
      content = '',
      close = config.props!.close
    }) {
      const container = document.createElement('div');
      const vnode = createVNode(Modal);
      render(vnode, container);
      const instance = vnode.component;
      document.body.appendChild(container);
        
      // 获取实例的 props ，进行传递 props
      const { props } = instance;

      Object.assign(props, {
        isTeleport: false,
        // 在父组件上我们用 v-model 来控制显示，语法糖对应的 prop 为 modelValue
        modelValue: true,
        title,
        content,
        close
      });
    }
  };
};
export default Modal;
```

### 如何监听事件

在Modal中已经写好的事件：
```javascript
// Modal.vue
setup(props, ctx) {
  let instance = getCurrentInstance();
  onBeforeMount(() => {
    instance._hub = {
      'on-cancel': () => {},
      'on-confirm': () => {}
    };
  });

  const handleConfirm = () => {
    ctx.emit('on-confirm');
    instance._hub['on-confirm']();
  };
  const handleCancel = () => {
    ctx.emit('on-cancel');
    ctx.emit('update:modelValue', false);
    instance._hub['on-cancel']();
  };

  return {
    handleConfirm,
    handleCancel
  };
}
```
我们可以看到在 上面的 setup 方法内部，获取了当前组件的实例，在组件挂载前，我们擅自添加了一个属性 _hub（且叫它事件处理中心吧~），并且添加了两个空语句方法 on-cancel，on-confirm，且在点击事件里都有被对应的调用到了。
再看看确认事件的具体实现，这里实现了点击确定，如果确定事件是一个异步操作，那我们需要在确定按钮上显示 loading 图标，且禁用按钮，来等待异步完成。
```javascript
// index.ts
app.config.globalProperties.$modal = {
  show({
    /* 其他选项 */
    onConfirm,
    onCancel
  }) {
    /* ... */

    const { props, _hub } = instance;
    
    const _closeModal = () => {
      props.modelValue = false;
      container.parentNode!.removeChild(container);
    };
    // 往 _hub 新增事件的具体实现
    Object.assign(_hub, {
      async 'on-confirm'() {
        if (onConfirm) {
          const fn = onConfirm();
          // 当方法返回为 Promise
          if (fn && fn.then) {
            try {
              props.loading = true;
              await fn;
              props.loading = false;
              _closeModal();
            } catch (err) {
              // 发生错误时，不关闭弹框
              console.error(err);
              props.loading = false;
            }
          } else {
            _closeModal();
          }
        } else {
          _closeModal();
        }
      },
      'on-cancel'() {
        onCancel && onCancel();
        _closeModal();
      }
    });

    /* ... */

  }
};
```
其他内容请看原文，如国际化、与ts的结合。

# 彩蛋

今天没有彩蛋，还好没停更。