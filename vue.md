view 层：
model 层：
vueModel 层：视图模型层，是 view 和 model 沟通的桥梁，
             一方面是数据绑定，将model的改变实时的反应到view中；
             另一方面是DOM监听，当DOM发生一些事件时，可以检讨到，并在需要的情况下改变对应的data.

el：类型：string | HTMLElement
    作用：决定之后vue实例会管理哪个DOM
data:  类型：Object | Function ( 组件当中data必须是一个函数 )
       作用：vue 实例对应的数据对象
methods: 类型：{ [key:string]:Function }
         作用：定义属于vue的一些方法，可以在其他地方调用，也可以在指令中使用

vue的生命周期函数又称钩子函数

Mustache 语法（双大括号）  v-text 指令（通常接受一个string类型，没有该语法灵活）与该语法作用一致

v-pre：用于跳过这个元素和它的子元素的编译过程，用于显示原本的Mustache语法。

在vue解析之前，div中有一个属性 v-cloak ；在vue解析解析之后，div中没有该属性

在某些情况下，我们可能不希望界面随意的跟随值得变化而改变：
    v-once : 该指令后面不需要跟任何表达式；
             该指令表示元素和组件只渲染一次，不会随数据的改变而改变


在某些情况下，我们从服务器请求到的数据本身就是一个HTML代码：
    v-html : 该指令后面往往会跟上一个string类型；
             会将string的html解析出来并且进行渲染


v-bind 指令：作用：动态绑定属性；
             缩写： :
  v-bind 指令绑定 class 的值：   普通class 的值和绑定的 class 可以同时共存
        对象语法：  {类名1: true, 类名2: boolean}  （属性名不需要加 单引号）通过控制true和false来展示哪种样式
        数组语法：  ['类名1','类名2'] 该类名是字符串，则为自己定义的类名
                    [类名1,类名2]      该类名是变量
  v-bind 指令绑定 style 的值：
        对象语法： {key(css的属性名): value(属性值，值可来自于data中的属性)}
        数组语法：[属性名，属性名]  多个值，以逗号分割即可


computed：计算属性
   每个计算属性都包含一个getter和setter;一般我们只是使用getter来读取。也可以提供一个setter方法（不常用）
   计算属性的本质：
       computed: {
          fullName:{
            set: function(newValue){
                // 修改时会调用该方法
            },
            get: function(){
               // 返回页面所需的值
            }
          }
       }

computed 和 methods 都可以进行计算属性，但是 computed 会进行缓存，若多次使用，其只会调用一次。

事件监听： v-on 指令： 作用：绑定监听事件；
                       缩写：@ ；

  当通过methods中定义方法，以供 @click 调用时，需要注意参数问题：
     1.如果该方法不需要额外参数，那么方法后的 () 可以不添加；若方法本身中有一个参数，则默认将原生事件event参数传递进去。
     2.如果需要同时传入某个参数，同时需要event时，可以通过 $event 传入事件。
  v-on 修饰符：
     1. .stop修饰符  阻止事件冒泡
     2. .prevent修饰符  阻止默认事件
     3. .{keyCode(键修饰符)|keyAlias(键别名)} 监听某个键盘的点击事件
     4. .native 监听组件根元素的原生事件
     5. .once修饰符 只触发一次回调


v-if 与 v-show 的区别：
   v-if  当条件为false时，包含v-if指令的元素，不会存在于dom元素中
   v-show 当条件为false时，只是在元素上增加了display:none;


v-model 指令用来实现表单元素和数据的双向绑定
   该指令其实是一个语法糖，其本质上包含两个操作：
      1.v-bind 绑定一个value属性
      2.v-on 指令给当前元素绑定input事件
   例： <input type="text" v-model="message"> 
        等同于
        <input type="text" v-bind:value="message" v-on:input="message = $event.target.value">

v-model 的 修饰符：
   lazy 修饰符 ：
         默认情况下，v-model默认在input事件中同步输入框的数据的，一旦数据发生变化，对应的data中的数据就会自动发生过变；
         lazy 修饰符可以让数据在失去焦点或者回车的时候才会更新。
   
   number 修饰符 ：
         默认情况下，在输入框中我们无论输入字母还是数字，都会被当作字符串类型进行处理；
         number 修饰符可以让在输入框中输入的内容自动转换为数字类型。
   
   trim 修饰符 ：      
         如果输入的内容首尾有很多空格，通常我们希望将其去除；
         trim修饰符可以过滤内容左右两边的空格


Vue 中的组件化：
   组件的使用分成三个步骤： 
      1.创建组件构造器；  调用Vue.extend()方法创建组件构造函数
      2.注册组件；   调用Vue.component()方法注册组件
      3.使用组件     在Vue实例的作用范围内使用组件    

Vue模版分离写法： 1.script标签实现(type值必须为"text/x-template")    2.template标签实现      

组件不可用访问Vue实例data中的数据。组件内部有自己保存数据的属性data，其内部data属性的值必须是一个函数，而这个函数必须返回一个对象，对象内部保存着数据。
组件内部data必须是一个函数的原因：每个组件实例都有自己的状态，互不影响。

父传子 props的值有两种方式：
   1.字符串数组，数组中的字符串就是传递时的名称
   2.对象，对象可以设置传递时的类型，也可以设置默认值等
父组件向子组件传值时的参数名不可以用 驼峰标识 。   

子传父 ： 子组件通过this.$emit自定义事件，触发父组件事件；父组件中通过v-on来监听组件事件

父子组件的访问方式：
   父组件访问子组件：使用 $children 或 $refs
   子组件访问父组件：使用$parent

slot(插槽):
   slot-scope="slot"  获取子组件中插槽对象，获取插槽中的数据


常见的模块化规范： commonjs  AMD  CMD  ES6的Modules
