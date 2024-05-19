<template>
    <div class="person">

        姓:<input type="text" v-model="firstName">
        <br>
        名:<input type="text" v-model="lastName">
        <br>
        <!-- 全名:<span>{{firstName.slice(0,1).toUpperCase()+ firstName.slice(1)}}-{{lastName}}</span> -->
            <span>{{fullName}}</span>
        <br>
        <button @click="changeFullName">将全名改成li-si</button>


        <h2>汽车信息：一辆{{car.brand}}车，价值{{car.price}}万</h2>
        <button @click="changePrice">修改汽车的价格</button>
        <button @click="changeCar">修改汽车</button>
        <br>
        <h2>游戏列表：</h2>
        <ul>
            <li v-for="g in games" :key="g.id">{{g.name}}</li>
        </ul>
        <button @click="changeFirstGame">修改第一个游戏的名字</button>
        <br>
        <h2>数字:{{obj.a.b.c}}</h2>
        <button @click="changeObj">修改数字</button>
        <!-- <h2>a = {{a}} </h2> -->
        <h2>姓名: {{name}} </h2>
        <h2>年龄: {{age}} </h2>
        <h2>地址: {{address}} </h2>
        <button @click="changeName">修改名字</button>
        <button @click="changeAge">修改年龄</button>
        <button @click="showTel">查看联系方式</button>

        <!-- <br>
        <h2>测试1:{{a}}</h2>
        <h2>测试2:{{c}}</h2>
        <button @click="b">测试</button> -->
    </div>
</template>

<script lang="ts">
    // JS或TS
    export default {
        name: 'Person',
        // beforeCreate(){
        //     console.log('beforeCreate')
        // },
        // data(){
        //     return {
        //         a: 100,
        //         c: this.name
        //     }
        // },
        // methods: {
        //     b(){
        //         console.log('b')
        //     }
        // },
        /* setup(){
            // console.log('@@', this) // setup函数中的this是undefined,vue3中已经弱化this了
            // 数据，原来是写在data中的，此时的name、age、tel都不是响应式的数据
            let name = '张三'
            let age = 18
            let tel = '13751559890'

            //方法
            function changeName(){
                name = 'zhang-san' //注意：这样修改name，页面是没有变化的
                console.log(name)   //name确实改了，但name不是响应式的
            }
            function changeAge(){
                age += 1    //注意：这样修改age，页面是没有变化的
                console.log(age) //age确实改了，但age不是响应式的
            }
            function showTel(){
                alert(tel)
            }

            //将数据、方法交出去，模版中才可以使用
            return {name,age,changeName,changeAge,showTel}

            //setup的返回值也可以是一个渲染函数
            //return ()=> '哈哈'
        }*/
    }
</script>

<script lang="ts" setup name="Person">
    import {ref,computed} from 'vue' //可以定义基本类型响应式、对象类型
    import {reactive, toRefs, toRef} from 'vue' //只能定义对象类型变成响应式,toRefs取多个，toRef取一个

    let firstName = ref('zhang')
    let lastName = ref('san')

    //这么定义的fullName是一个计算属性，只可以读不可以修改
    // let fullName = computed(()=>{
    //     return firstName.value.slice(0,1).toUpperCase()+ firstName.value.slice(1) + '-' + lastName.value
    // })

    //这么定义的fullName是一个计算属性，可读可以修改
    let fullName = computed({
        get(){
            return firstName.value.slice(0,1).toUpperCase()+ firstName.value.slice(1) + '-' + lastName.value
        },
        set(val){
            const [str1, str2] = val.split('-')
            firstName.value = str1
            lastName.value = str2
        }
    })

    // let age = ref(18)

    //let{name, age} = toRefs(person)

    let car = reactive({brand: '奔驰', price: 100})

    let games = reactive([
        {id: 'gamesid01', name: '王者荣耀'},
        {id: 'gamesid02', name: '喷射战士'},
        {id: 'gamesid03', name: '第五人格'}
    ])

    let obj = ref({
        a:{
            b:{
                c:666
            }
        }
    })

    console.log(car);
    

    let a = 666
    let name = ref('张三')
    let age = ref(8)
    let tel = '13751559890'
    let address = '广东省惠州市惠阳区'

    console.log(1,name);
    console.log(2,age);
    console.log(3,tel);
    console.log(4,address);
    
    //npm i vite-plugin-vue-setup-extend -D

    //方法

    function changeFullName(){
        fullName.value = 'li-si'
    }
    function changeObj(){
        obj.value.a.b.c = 777
    }
    function changeFirstGame(){
        games[0].name = '原神'
    }
    /**
     * 调整汽车价格
     * 该函数无参数。
     * 该函数没有返回值。
     */
    function changePrice(){
        // 增加汽车价格10元
        car.price += 10
    }
    function changeCar(){
        Object.assign(car,{brand:'宝马',price:50})  //前面是reactive
        // car.value = {brand:'宝马',price:50} 前面是ref
    }
    function changeName(){
        name.value = 'zhang-san' //注意：这样修改name，页面是没有变化的
        console.log(name.value)   //name确实改了，但name不是响应式的
    }
    function changeAge(){
        age.value += 1    //注意：这样修改age，页面是没有变化的
        console.log(age.value) //age确实改了，但age不是响应式的
    }
    function showTel(){
        alert(tel)
    }
</script>

<style>
   /* 样式 */
   .person{
    background-color:skyblue;
    box-shadow: 0 0 10px;
    border-radius: 10px;
    padding: 20px;
   }
   button{
    margin: 0 5px;
   }
   li{
    font-size: 20px;
   }
</style>