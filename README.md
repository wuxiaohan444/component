#父子组件的通信
1. 父-->子 （通过属性）
    +   1.1 在子组件中声明props，是个数组，在数组中声明用来接收数据的属性的名称
    +   1.2 在父组件中使用子组件的时候，通过在子组件标签中添加 之前声明好的属性，给属性赋值类向子组件传递数据
    +   1.3 在子组件内，就可以像使用正常数据一样使用通过props声明从来的数据
    +   这个数据传递过来之后，是单向绑定的，父组件中发生改变，子组件会相应的改变，但是子组件中不允许改变这个数据，就算能改变，父组件中也不会受到影响！
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
    <div id="app">
        <mycomp :msgfather="msg"></mycomp>
    </div>
    <script src="node_modules/vue/dist/vue.js"></script>
    <script>
        Vue.component("mycomp", {
            template: "<div>{{msgfather}} </div>",
            data() {
                return {
                    msg: "这是儿子"
                }
            },
            props: ["msgfather"]
        })
    
        var vm = new Vue({
            el: '#app',
            data: {
                msg: '这是父亲'
            }
        })
    
    </script>
    </body>
    </html>
    ```
    
2. 子-->父 （通过事件）
    本质： 其实就间接的在子组件中调用父组件的方法
    +   1.1 在父组件中声明方法
    +   1.2 在父组件使用子组件的时候，通过事件绑定，将这个方法绑定给子组件 @
    +   1.3 在子组件中，如果想要和父组件通信，只需要触发之前注册好的事件即可(this.$emit)
    +   注意： $emit在触发事件的时候，是可以传递参数的，这个参数可以直接在父组件中的函数的形参中接收，但是要注意，只能接收一个参数，所以如果数据较多，则利用对象传递
    
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script src="node_modules/vue/dist/vue.js"></script>
    </head>
    <body>
    <div id="app">
        <div>
            <mycomp @sendmsg="getson"></mycomp>
        </div>
    </div>
    <script>
        Vue.component('mycomp',{
            template:"<div>儿子<button @click='send'>发送消息</button></div> ",
            data(){
                return{
                    msg:"这是儿子"
                }
            },
            methods:{
                send(){
                    this.$emit("sendmsg")
                }
            }
        })
    
        var vm=new Vue({
            el:'#app',
            data:{
                msg:''
            },
            methods:{
              getson(msg){
                  console.log('我儿子来信了'+msg)
              }
            }
        })
    </script>
    </body>
    </html>
    ```
    
    ##ps:补充一些兄弟通信
    两个兄弟组件因为是相互独立的，因此需要第三方来实现通信
   + $on是用来注册事件的
   +  $emit是用来触发事件的
   +    $on注册的事件，只能在当前实例中被$emit触发
   + 新建一个vue实例，这个实例可以用来注册事件以及触发事件     从而达到兄弟组件之间的通信效果

   ```javascript
         var messageHub = new Vue();
   
   
           Vue.component("bigbrother", {
               template: "<h1>哥哥组件</h1>",
               created(){
                   messageHub.$on("listenDdMsg", this.getMsgFromDd)
               },
               methods: {
                   getMsgFromDd(msg){
                       console.log("我家兄弟来信了， 弟弟说:" + msg);
                   }
               }
           })
   
           Vue.component("youngbrother", {
               template: "<div><h1>弟弟组件</h1><button @click='btnClick'>发送消息给哥哥</button></div>",
               methods: {
                   btnClick(){
                       messageHub.$emit("listenDdMsg", "我要结婚了，你快来上份子钱！！")
                   }
               }
           })
   
           var vm = new Vue({
               el: "#app",
               data: {
               }
           })

   ````
   

    
    
      
    
