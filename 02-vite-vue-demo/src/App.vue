<template>
  <div>
    <!-- 插值 -->
    <p>姓名：{{ name }}</p>
    <!-- 属性绑定 -->
    <img v-bind:src="imgUrl1" alt="">
    <!-- 事件 -->
    <button @click="changeName">修改名字</button>
    <!-- 双向绑定 -->
    <input v-model="name"/>
    <!-- 条件 -->
    <p v-if="isShow">显示内容</p>
    <!-- 循环 -->
    <ul>
      <li v-for="item in list" :key="item.id">{{ item.title }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
const name = ref("张三")
const imgUrl1 = ref("https://picsum.photos/100")
const isShow = ref(true)
const list = ref([
  {id:1, title:"Vue学习1"},
  {id:2, title:"Vue学习2"}
])

// 事件方法
function changeName(){
  name.value = "李四"
}

// 计算属性
const doubleName = computed(()=>{
  return name.value + "---" + name.value
})

// 侦听器
watch(name,(newVal,oldVal)=>{
  console.log("name发生变化",newVal,oldVal)
})
</script>

<style scoped>
</style>