<template>
  <section class="profile-card" :style="{ '--theme-color': themeColor }">
    <!-- 头像，src绑定状态 -->
    <img class="avatar" :src="user.avatar" :alt="user.name" />
    <h2 class="username">{{ user.name }}</h2>

    <!-- 角色标签：对象形式类绑定，禁止v-if链式 -->
        <div class="tag-row">
      <span
        class="role-tag"
        :class="{
          'role-organizer': user.role === 'organizer',
          'role-auditor': user.role === 'auditor',
          'role-student': user.role === 'student'
        }"
      >
        {{ ROLE_MAP[user.role] }}
      </span>

      <span class="duty-tag" :class="{ 'duty-off': !isOnDuty }">
        {{ isOnDuty ? '在岗' : '请假' }}
      </span>
    </div>

    <!-- 技能列表 v-for渲染 -->
    <ul class="skill-list">
      <li v-for="skill in skills" :key="skill">{{ skill }}</li>
    </ul>

    <!-- 交互按钮 -->
    <div class="btn-group">
      <button @click="isOnDuty = !isOnDuty">切换在岗状态</button>
      <button @click="themeColor = '#2f6fed'">蓝色主题</button>
      <button @click="themeColor = '#0f9d58'">绿色主题</button>
      <button @click="themeColor = '#e8710a'">橙色主题</button>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

// 用户信息状态
const user = ref({
  name: '刘德利',
  avatar: 'https://picgocloud.com/m/b699f281-f0c5-4081-ab67-19e8526c36c3.jpg',
  role: 'student'
})
// 是否在岗状态
const isOnDuty = ref(true)
// 主题色状态
const themeColor = ref('#2f6fed')
// 技能数组状态
const skills = ref(['需求梳理', '活动执行', '数据复盘'])

// 角色文本映射
const ROLE_MAP = {
  organizer: '活动组织者',
  auditor: '审核员',
  student: '学生'
}
</script>

<style scoped>
.profile-card {
  width: 360px;
  padding: 24px;
  border-radius: 16px;
  box-shadow: 0 2px 12px #00000020;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
}
.username {
  color: var(--theme-color);
}
.role-tag {
  display: inline-block;
  padding: 4px 8px;
  border-radius: 4px;
  color: white;
  margin: 0 6px;
}
.role-organizer {
  background: var(--theme-color);
}
.role-auditor {
  background: #6c5ce7;
}
.role-student {
  background: #00b894;
}
.duty-tag {
  padding:4px 8px;
  border-radius:4px;
  background:#27ae60;
  color:#fff;
}
.duty-off {
  background:#e74c3c;
}
.skill-list {
  list-style: none;
  padding: 0;
  display: flex;
  gap:8px;
  margin:12px 0;
}
.skill-list li {
  padding:4px 10px;
  background:#f1f1f1;
  border-radius:99px;
}
.btn-group {
  display:flex;
  gap:8px;
  flex-wrap:wrap;
  margin-top:16px;
}
button {
  padding:6px 10px;
  cursor:pointer;
}
.tag-row {
  display: flex;
  gap: 10px;
  align-items: center;
  justify-content: center;
}
</style>