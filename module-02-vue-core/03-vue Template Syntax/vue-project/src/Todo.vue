<script setup>
import { ref } from 'vue'

const todos = ref([
  { id: 1, text: '整理活动报名名单', done: false },
  { id: 2, text: '联系场地负责人', done: true },
  { id: 3, text: '准备审核说明材料', done: false }
])
const newText = ref('')

const doneCount = () => todos.value.filter(t => t.done).length

// 添加任务
const addTodo = () => {
  if (!newText.value.trim()) return
  const newId = Math.max(...todos.value.map(item => item.id), 0) + 1
  todos.value.push({
    id: newId,
    text: newText.value,
    done: false
  })
  newText.value = ''
}

// 删除任务
const delTodo = (id) => {
  todos.value = todos.value.filter(item => item.id !== id)
}
</script>

<template>
  <div class="todo-box">
    <h2>活动任务清单</h2>
    <!-- 添加任务区域 -->
    <div class="add-area">
      <input
        v-model="newText"
        placeholder="请输入新任务..."
        class="input-todo"
      >
      <button @click="addTodo" class="btn-add">添加任务</button>
    </div>

    <ul class="todo-list">
      <li v-for="todo in todos" :key="todo.id" class="todo-item">
        <input type="checkbox" v-model="todo.done" class="checkbox">
        <span
          :style="{ textDecoration: todo.done ? 'line-through' : 'none' }"
          class="todo-text"
        >
          {{ todo.text }}
        </span>
        <button @click="delTodo(todo.id)" class="btn-del">删除</button>
      </li>
    </ul>
    <p class="count-info">已完成 {{ doneCount() }} / {{ todos.length }}</p>
  </div>
</template>

<style>
.todo-box {
  width: 420px;
  margin: 30px auto;
  padding: 24px;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0,0,0,0.1);
  font-family: "Microsoft Yahei", sans-serif;
}
h2 {
  text-align: center;
  color: #333;
  margin-top: 0;
}
.add-area {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}
.input-todo {
  flex:1;
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 14px;
}
.btn-add {
  padding: 8px 16px;
  background: #42b983;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}
.btn-add:hover {
  background: #36a070;
}
.todo-list {
  list-style: none;
  padding:0;
}
.todo-item {
  display:flex;
  align-items:center;
  gap:10px;
  padding:10px 8px;
  border-bottom:1px solid #eee;
}
.todo-text {
  flex:1;
  font-size:15px;
  color:#444;
}
.btn-del {
  padding:5px 10px;
  background:#f56c6c;
  color:white;
  border:none;
  border-radius:4px;
  cursor:pointer;
}
.btn-del:hover {
  background:#dd5555;
}
.count-info {
  text-align:center;
  color:#666;
  margin-top:16px;
}
</style>