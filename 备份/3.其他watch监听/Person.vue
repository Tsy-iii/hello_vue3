<template>
    <div class="person">
        <h1>情况四:监视【ref】或【reactive】定义的【对象类型】数据中的某个属性</h1>
        <h2>姓名:{{person.name}}</h2>
        <h2>年龄:{{person.age}}</h2>
        <h2>汽车:{{person.car.c1}}、{{person.car.c2}}</h2>
        <button @click="changeName">修改名字</button>
        <button @click="changeAge">修改年龄</button>
        <button @click="changeC1">修改第一台车</button>
        <button @click="changeC2">修改第二台车</button>
        <button @click="changeCar">修改整个车</button>

        <h1>情况五:监视上述的多个数据</h1>
        <h2>姓名:{{person.name}}</h2>
        <h2>年龄:{{person.age}}</h2>
        <h2>汽车:{{person.car.c1}}、{{person.car.c2}}</h2>
        <button @click="changeName">修改名字</button>
        <button @click="changeAge">修改年龄</button>
        <button @click="changeC1">修改第一台车</button>
        <button @click="changeC2">修改第二台车</button>
        <button @click="changeCar">修改整个车</button>

    </div>
</template>

<script lang="ts" setup name = "Person">
   import {ref,watch,reactive} from 'vue'

   //数据
   let person = reactive({
       name:'张三',
       age:18,
       car:{
        c1:'奔驰',
        c2:'宝马'
       }
   })

   //方法
   function changeName(){
       person.name += '~'
   }
   function changeAge(){
       person.age += 1
   }
   function changeC1(){
       person.car.c1 = '奥迪'
   }
   function changeC2(){
       person.car.c2 = '大众'
   }
   function changeCar(){
       person.car = {
        c1:'雅迪',
        c2:'爱玛'
       }
   }

   //监听,情况四：监视响应式对象中的某个属性，且该属性是基本类型的，要写成函数式
   watch(()=>{return person.name},(newValue,oldValue)=>{
       console.log('监听到person.name变化了',newValue,oldValue)
   })

    //监听,情况四：监视响应式对象中的某个属性，且该属性是基本类型的，可以直接写，也能写成函数，更推荐写成函数
   watch(()=>person.car,(newValue,oldValue)=>{
       console.log('监听到person.car变化了',newValue,oldValue)
   },{
       deep:true
   })

   //监视，情况五:监视上述的多个数据
   watch([()=>person.name, ()=>person.car.c1],(newValue,oldValue)=>{
       console.log('监听到person变化了',newValue,oldValue)
   },{
       deep:true
   })


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