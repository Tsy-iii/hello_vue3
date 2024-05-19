<template>
    <div class="person">
        <h1>情况一:监视【ref】定义的【基本类型】数据</h1>
        <h2>当前求和为:{{ sum }}</h2>
        <button @click="changeSum">点我sum+1</button>

        <h1>情况二:监视【ref】定义的【对象类型】数据</h1>
        <h2>姓名: {{ person.name }}</h2>
        <h2>年龄: {{ person.age }}</h2>
        <button @click="changeName">修改名字</button>
        <button @click="changeAge">修改年龄</button>
        <button @click="changePerson">修改整个人</button>

        <h1>情况三:监视【reactive】定义的【对象类型】数据</h1>
        <h2>姓名: {{ person1.name }}</h2>
        <h2>年龄: {{ person1.age }}</h2>
        <button @click="changeName1">修改名字</button>
        <button @click="changeAge1">修改年龄</button>
        <button @click="changePerson1">修改整个人</button>

    </div>
</template>

<script lang="ts" setup name="Person">
import { ref, watch, reactive } from 'vue'

//数据
let sum = ref(0)
//数据
let person = ref({
    name: '张三',
    age: 18
})

let person1 = reactive({
    name: '张三',
    age: 18
})

//方法
function changeSum() {
    sum.value += 1
}
function changeName() {
    person.value.name += '~'
}
function changeAge() {
    person.value.age += 1
}
function changePerson() {
    person.value = {
        name: '李四',
        age: 90
    }
}

//方法1
function changeName1() {
    person1.name += '~'
}
function changeAge1() {
    person1.age += 1
}
function changePerson1() {
    Object.assign(person1, {
        name: '李四',
        age: 90
    })
}

//监视,情况一:监视【ref】定义的【基本类型】数据
const stopWatch = watch(sum, (newValue, oldValue) => {
    console.log('sum变化了', newValue, oldValue)
    if (newValue >= 10) {
        alert('sum大于10')
        stopWatch()
    }
})

//监视,情况二:监视【ref】定义的【对象类型】数据,监视的是对象的地址值，若想监视对象内部的属性，需要手动开启深度监视
//watch的第一个参数是:被监视的数据
//watch的第二个参数是:监视的回调
//watch的第三个参数是:配置对象（deep、immediate等等）
watch(person, (newValue, oldValue) => {
    console.log('person变化了', newValue, oldValue)
}, {
    deep: true,
    immediate: true
})

//监视,情况三:监视【reactive】定义的【对象类型】数据,且默认是开启深度监视的
watch(person1, (newValue, oldValue) => {
    console.log('person1变化了', newValue, oldValue)
}, {
    // deep:true,
    immediate: true
})

</script>

<style>
/* 样式 */
.person {
    background-color: skyblue;
    box-shadow: 0 0 10px;
    border-radius: 10px;
    padding: 20px;
}

button {
    margin: 0 5px;
}

li {
    font-size: 20px;
}
</style>