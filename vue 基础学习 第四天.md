# 32 组件
* 什么是组件： 用我自己的话理解就是 “可以复用的、易于管理的div”。 => 一个页面需要轮播图，轮播图写成一个组件，如果需要复用在另一个页面载入该组件即可。用 Vue 开发的网页就是由不同的组件组成的。（之前都是写无数个div划分区域，现在写无数个组件然后组装成网页）
* 基础代码
```
<div id="app">
    <!-- 定义好全局组件之后， -->
    <test1></test1>
    <test2></test2>
    <test3></test3>
    <test4></test4>
</div>


<script>
    // 我们在这里定义 “全局组件”
    Vue.component('test1', { // Vue.component('标签', { //... 定义 })
        template: '<h1> 这是全局组件的模板test1 </h1>', //模板
    });

    // 为了方便我们还可以定义一个变量存储局部组件然后在根组件中载入 （这个定义也必须声明在前面）
    var test4= {
        template: '<h2> 这是局部组件的模板test4 </h2>',
    }

    // 我们之前一直定义的其实是 “根组件”
    var app = new Vue({
        el: '#app',
        data: {
            
        },
        // 我们还可以定义 “局部组件”
        components: {
            test3: {
                template: '<h2> 这是局部组件的模板test3 </h2>',
            },
            // test4: test4,
            // 可以使用 ES6 语法，由于 key: value 是一样的，直接:
            test4
        }
    });

    // “全局组件” 必须在根组件之前定义
    Vue.component('test2', {
        template: '<h1> 这是全局组件的模板test2 </h1>',
    });
</script>
```

* 定义全局组件 `Vue.component('这里对应载入时在html代码中写的标签名', { //这里面写具体定义 })`
* 全局组件 **必须** 在根组件前面声明。
* 定义局部组件，写在根组件的 **components** 里面。 `标签名: { //...定义 }`。
* 也可以在根组件前面声明并用变量存储起来，然后直接在 **components** 中载入该变量即可。
* ES6的语法，如果 json 的 键值对， key的名字和value的名字一样，可以直接写 key ，不写 value 。

# 33 组件中定义 data 数据
* 代码
```
 <div id="app">
    <test></test>
</div>

<!-- 在 Vue 中可以使用这样的方式定义模板 -->
<script type="text/x-template" id="myComponent">
    <ul>
        <li v-for="v in news">{{ v.id }} - {{ v.title }}</li>
    </ul>
</script>

<script>
    var myComponent = {
        template: "#myComponent",
        // data: {} //子组件中不能把data定义成属性。会报错：
        // The "data" option should be a function that returns a per-instance value in component definitions.
        data() { //必须这样定义 data() { return {json对象} }
            return {
                news: [
                    {id:1, title:'测试1'},
                    {id:2, title:'测试2'},
                ],
            };
        }
    };
    var app = new Vue({
        el: '#app',
        data: {

        },
        components: {
            // 有个坑： ES6 方式注册子组件不能使用 驼峰 风格命令，Vue会自动转换成全小写。
            test: myComponent,
        }
    });
</script>
```

* 我们是在外面定义的子组件，然后在根组件的 components 中注册。
* 在注册的时候发现了一个问题： 用ES6语法注册时，我将组件名定义为 "myComponent" ，Vue 解析为 "mycomponent"。没办法注册。
* 在子组件中定义data必须使用 `data() { //json对象 }` 这样的语法，而不能直接写 `data: {}`。
* 可以使用 `<script type="text/x-template" id="test"></script>` 来定义模板。然后再子组件定义中使用模板 `template: "#test"`

# 34 父组件给子组件传递参数
* 代码
```
<div id="app">
    <!-- 在标签里面传递 :子组件.props中声明的名字 = "根组件.data中的变量名" -->
    <!-- 如果不写 :属性="value" 的话，会解析为字符串 -->
    <students :students="students" flag1="false" :flag2="true"></students>    
</div>

<!-- 模板 -->
<script type="text/x-template" id="students">
    <ul>
        <li v-for="student in students"> {{ student.id }} - {{ student.name }} </li>

        <span v-if="flag1"> 这其实是字符串 {{ flag1 }} </span> <!-- 这里之所以flag = "false" 会显示是因为 字符串"false" = 布尔true -->
        <span v-if="flag2"> ：这才是布尔值 {{ flag2 }} </span> <!-- 因此这里如果传递 flag2 = false 则不会显示 -->
    </ul>
</script>

<script>
    // 子组件
    var students= {
        template: "#students",
        // 在接收时需要在 props 属性中声明
        props: ['students', 'flag1', 'flag2'],
    };
    
    // 根组件
    var app = new Vue({
        el: '#app',
        // 定义父组件的数据
        data: {
            students: [
                {id: 1, name: 'liuihaoyu'},
                {id: 2, name: 'lidaye'},
                {id: 3, name: 'linainai'},
            ],
        },
        // 注册子组件
        components: {
            students,
        }
    });
</script>
```

* 整个过程：（根组件）父组件.data中定义数据，然后在html代码中 **子组件的标签上** 使用 `:xxx="定义的变量"` 传递数据。
* 子组件需要声明 `props = ['接受的数据xxx', '接受的数据yyy', '接受的数据zzz']`
* 如果在html中， 子组件的标签上这么传递数据 `xxx="value"` ，这样 子组件.props 接受的其实是字符串，而不是父组件data中声明的值。 