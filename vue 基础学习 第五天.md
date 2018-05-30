# 41 css 动画
* 使用 [animate.css](https://daneden.github.io/animate.css/)
* 代码
```
<!-- 1、引用 animate.css -->
<link rel="stylesheet" href="animate.css">

 <div id="app">
    <button @click="flag=!flag">切换</button>
    <!-- 2、用一个 <transition> 标签把动画影响的元素包裹起来 -->
    <!-- 3、给 <transition> 属性 enter-active-class="指定元素显示时的动画" leave-active-class="元素隐藏时的动画" -->
    <transition enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
        <h1 v-if="flag">测试</h1>
    </transition>
</div>

<script>
    var app = new Vue({
        el: '#app',
        data: {
            flag: true,
        },
    });
</script>
```
* 使用 `<transition>` 标签将要受到动画影响的元素包起来
* 在 `<transition>` 标签中指定属性 `enter-active-class="显示过程动画样式类"`...
* 一共有这么几个属性：
```
# enter="开始进入的样式"
# enter-active="进入过程中"
# enter-to="进完成后"
# leave-active="离开过程中"
# leave="完全离开后"
```