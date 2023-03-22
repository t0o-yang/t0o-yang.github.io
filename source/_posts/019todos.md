# 前言

日志，各位看官就当乐子看吧。

**正经人谁写日记啊？！！**    ——鹅城县长

# todos页面

今天的`todos`项目有大进展，终于把大致样子构造出来了，但是有个大问题，列表上的前后按钮是直接用了`uni-easyinput`的`prefixIcon`和`suffixIcon`，虽然组件上有`iconClick`事件监听前后按钮点击，但是在得知点击的是前后哪个按钮后，无法传入其他参数，也就无法改变事项的状态了。

<img src="https://gitee.com/t0o-yang/Images/raw/e60923799c94481b5a695d6b89930ffdbdd5b6f1/localhost_5173_(iPhone%20SE).png" title="" alt="todos页面" style="zoom:30%;" data-align="center">

# 总结

组件好用，但是当需要调整时，也是很难改的
