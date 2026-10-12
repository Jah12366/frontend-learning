<script setup>
import { ref } from 'vue'

const isSignedUp = ref(false)
const count = ref(0)

const handleClick = () => {
  isSignedUp.value = !isSignedUp.value
  count.value ++;
}
</script>

<template>
  <button @click="handleClick" class="btn">
    {{ isSignedUp ? '退订' : '报名' }}
  </button>

  <p :class="isSignedUp ? 'status--done' : 'status--idle'">
    {{ isSignedUp ? '已报名' : '未报名' }}
  </p>

  <p class="show">
    你已经点击了 {{ count }} 次
  </p>

  <p class="show">hello world</p>
</template>

<style>
.btn{
  padding: 10px 30px;
  background-color: #5CA67B;
  color: #FFFFFF;
  border: none;
  border-radius: 8px;
}
p {
  width: 200px;
  height: 80px;
  line-height: 80px;
  text-align: center;
  margin-top: 10px;
}

.show {
  background-color: antiquewhite;
}

.status--done {
  background-color: #5CA67B;
  color: #FFFFFF;
}
</style>