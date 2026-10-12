<script setup>
import { ref } from 'vue'

// 布尔值，控制切换海报
const isNext = ref(false)

// 第一张海报
const activity1 = ref({
  title: '2026 春季校园歌手大赛',
  status: 'signing', // draft / signing / closed / finished
  cover: 'https://picgocloud.com/m/05a2bddf-d441-49b3-aa1d-4f3fbb3056a9.png',
  offline: false
})

// 第二张海报
const activity2 = ref({
  title: '校园街舞挑战赛',
  status: 'draft',
  cover: 'https://picgocloud.com/m/83529c85-45ab-4886-88e8-907cdcf3d952.png',
  offline: false
})

// 当前显示的活动（计算属性效果，用布尔判断）
const currentActivity = ref(activity1.value)

// 下一张
const next = () => {
  isNext.value = true
  currentActivity.value = activity2.value
}
// 上一张
const prev = () => {
  isNext.value = false
  currentActivity.value = activity1.value
}
</script>

<template>
  <article class="activity-card">
    <img :src="currentActivity.cover" :alt="currentActivity.title" />
    <h3>{{ currentActivity.title }}</h3>

    <span class="tag" :class="{ 'tag--signing': currentActivity.status === 'signing' }">
      {{ currentActivity.status === 'signing' ? '报名中' : currentActivity.status === 'draft' ? '草稿' : '已结束' }}
    </span>

    <button :disabled="currentActivity.status !== 'draft'">下架</button>
    <!-- 按钮：用布尔值isNext判断是否禁用 -->
    <button @click="prev" :disabled="!isNext">上一张</button>
    <button @click="next" :disabled="isNext">下一张</button>
  </article>
</template>

<style>
.activity-card {
  width: 320px;
}
.activity-card img {
  width: 100%;
  border-radius: 8px;
}
.tag {
  display: inline-block;
  padding: 3px 8px;
  background: #ccc;
  border-radius:4px;
}
.tag--signing {
  background: #42b983;
  color:white;
}
button {
  margin:4px;
}
</style>