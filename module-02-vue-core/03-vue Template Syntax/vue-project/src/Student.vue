<script setup>
import { ref } from "vue";
const students = [
  {
    id: "20230101",
    name: "林一鸣",
    className: "软件 2301",
    direction: "前端开发",
    score: 92,
    status: "active",
  },
  {
    id: "20230102",
    name: "周予安",
    className: "软件 2301",
    direction: "后端开发",
    score: 85,
    status: "active",
  },
  {
    id: "20230103",
    name: "苏晚晴",
    className: "软件 2302",
    direction: "前端开发",
    score: 78,
    status: "intern",
  },
  {
    id: "20230104",
    name: "陈砚舟",
    className: "软件 2302",
    direction: "数据开发",
    score: 64,
    status: "leave",
  },
  {
    id: "20230105",
    name: "顾星野",
    className: "软件 2302",
    direction: "后端开发",
    score: 88,
    status: "active",
  },
  {
    id: "20230106",
    name: "许知微",
    className: "软件 2303",
    direction: "测试开发",
    score: 71,
    status: "intern",
  },
  {
    id: "20230107",
    name: "沈墨白",
    className: "软件 2303",
    direction: "前端开发",
    score: 95,
    status: "active",
  },
  {
    id: "20230108",
    name: "叶清和",
    className: "软件 2303",
    direction: "数据开发",
    score: 59,
    status: "leave",
  },
];

const viewMode = ref("table");
const onlyPass = ref(false);

// ========== 新增弹窗相关ref ==========
const dialogVisible = ref(false);
const selectedStudent = ref(null);

const STATUS_MAP = {
  active: { text: "在读", type: "success" },
  intern: { text: "实习中", type: "warning" },
  leave: { text: "休学", type: "info" },
};

function scoreColor(score) {
  if (score >= 85) return "#0f9d58";
  if (score >= 70) return "#e6a23c";
  return "#f56c6c";
}

// 修改详情函数：赋值选中学生、打开弹窗
function showDetail(student) {
  selectedStudent.value = student;
  dialogVisible.value = true;
}

function getFilterStudents() {
  const result = [];
  for (let i = 0; i < students.length; i++) {
    const s = students[i];
    if (!onlyPass.value || s.score >= 60) {
      result.push(s);
    }
  }
  return result;
}

const directionList = ["前端开发", "后端开发", "数据开发", "测试开发"];
function getStudentsByDirection(dirName) {
  const arr = [];
  const filterList = getFilterStudents();
  for (let i = 0; i < filterList.length; i++) {
    const s = filterList[i];
    if (s.direction === dirName) {
      arr.push(s);
    }
  }
  return arr;
}
</script>

<template>
  <div class="page">
    <header class="page__head">
      <div>
        <h2 class="page__title">学生名单</h2>
        <p class="page__desc">
          共 {{ getFilterStudents().length }} 人 · 切换视图用的是 v-if / v-else-if / v-else
        </p>
      </div>
      <div class="header-right">
        <div class="switch-wrap">
          <span>只看及格</span>
          <el-switch v-model="onlyPass" />
        </div>
        <el-radio-group v-model="viewMode">
          <el-radio-button value="table">表格</el-radio-button>
          <el-radio-button value="card">卡片</el-radio-button>
          <el-radio-button value="list">名单</el-radio-button>
          <el-radio-button value="group">按方向分组</el-radio-button>
        </el-radio-group>
      </div>
    </header>

    <!-- 1 表格模式 -->
    <el-table v-if="viewMode === 'table'" :data="getFilterStudents()" stripe border>
      <el-table-column prop="id" label="学号" width="120" />
      <el-table-column prop="name" label="姓名" width="110" />
      <el-table-column prop="className" label="班级" width="120" />
      <el-table-column prop="direction" label="方向" />
      <el-table-column prop="score" label="成绩" width="100" sortable>
        <template #default="scope">
          <span class="score" :style="{ color: scoreColor(scope.row.score) }">{{
            scope.row.score
          }}</span>
        </template>
      </el-table-column>
      <el-table-column label="状态" width="110">
        <template #default="scope">
          <el-tag :type="STATUS_MAP[scope.row.status].type" effect="light">
            {{ STATUS_MAP[scope.row.status].text }}
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="90">
        <template #default="scope">
          <el-button link type="primary" @click="showDetail(scope.row)">查看</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 2 卡片模式 -->
    <el-row v-else-if="viewMode === 'card'" :gutter="16">
      <el-col
        v-for="item in getFilterStudents()"
        :key="item.id"
        :xs="24"
        :sm="12"
        :md="8"
        class="card-col"
      >
        <el-card shadow="hover" class="stu-card">
          <div class="stu-card__top">
            <el-avatar :size="48" class="avatar">{{
              item.name.charAt(0)
            }}</el-avatar>
            <div>
              <p class="stu-card__name">{{ item.name }}</p>
              <p class="stu-card__id">{{ item.id }} · {{ item.className }}</p>
            </div>
          </div>
          <div class="stu-card__tags">
            <el-tag size="small" effect="plain">{{ item.direction }}</el-tag>
            <el-tag size="small" :type="STATUS_MAP[item.status].type" effect="light">
              {{ STATUS_MAP[item.status].text }}
            </el-tag>
          </div>
          <p class="stu-card__score">成绩 {{ item.score }}</p>
          <el-progress
            :percentage="item.score"
            :color="scoreColor(item.score)"
            :stroke-width="8"
            :show-text="false"
          />
          <template #footer>
            <el-button link type="primary" @click="showDetail(item)">查看详情</el-button>
          </template>
        </el-card>
      </el-col>
    </el-row>

    <!-- 3 按专业方向分组视图 -->
    <div v-else-if="viewMode === 'group'" class="group-view">
      <div v-for="dir in directionList" :key="dir" class="group-block">
        <h3 class="group-title">{{ dir }}</h3>
        <ul class="group-list">
          <li
            v-for="stu in getStudentsByDirection(dir)"
            :key="stu.id"
            class="group-item"
          >
            <el-avatar :size="32" class="avatar">{{ stu.name.charAt(0) }}</el-avatar>
            <span class="g-name">{{ stu.name }}</span>
            <span class="g-meta">{{ stu.id }} · {{ stu.className }}</span>
            <span class="score" :style="{ color: scoreColor(stu.score) }">
              {{ stu.score }}
            </span>
            <el-tag size="small" :type="STATUS_MAP[stu.status].type" effect="light">
              {{ STATUS_MAP[stu.status].text }}
            </el-tag>
            <el-button link type="primary" size="small" @click="showDetail(stu)">查看</el-button>
          </li>
        </ul>
      </div>
    </div>

    <!--4 名单模式 -->
    <ul v-else class="name-list">
      <li v-for="item in getFilterStudents()" :key="item.id" class="name-list__item">
        <el-avatar :size="32" class="avatar">{{ item.name.charAt(0) }}</el-avatar>
        <span class="name-list__name">{{ item.name }}</span>
        <span class="name-list__meta"
          >{{ item.id }} · {{ item.className }} · {{ item.direction }}</span
        >
        <span class="score" :style="{ color: scoreColor(item.score) }">{{
          item.score
        }}</span>
        <el-tag size="small" :type="STATUS_MAP[item.status].type" effect="light">
          {{ STATUS_MAP[item.status].text }}
        </el-tag>
        <el-button link type="primary" size="small" @click="showDetail(item)">查看</el-button>
      </li>
    </ul>

    <!-- ========== 新增对话框 el-dialog ========== -->
    <el-dialog v-model="dialogVisible" title="学生详情" width="450px">
      <div v-if="selectedStudent" class="detail-box">
        <p><b>姓名：</b>{{ selectedStudent.name }}</p>
        <p><b>学号：</b>{{ selectedStudent.id }}</p>
        <p><b>班级：</b>{{ selectedStudent.className }}</p>
        <p><b>专业方向：</b>{{ selectedStudent.direction }}</p>
        <p><b>成绩：</b>{{ selectedStudent.score }}</p>
        <p><b>在读状态：</b>
          <el-tag :type="STATUS_MAP[selectedStudent.status].type">
            {{ STATUS_MAP[selectedStudent.status].text }}
          </el-tag>
        </p>
      </div>
      <template #footer>
        <el-button @click="dialogVisible = false">关闭</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<style scoped>
.page {
  max-width: 1000px;
  margin: 40px auto;
  padding: 0 16px;
}
.page__head {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 16px;
}
.header-right{
  display:flex;
  align-items:center;
  gap:16px;
}
.switch-wrap{
  display:flex;
  align-items:center;
  gap:8px;
}
.page__title {
  margin: 0;
  font-size: 22px;
  color: #1f2329;
}
.page__desc {
  margin: 6px 0 0;
  font-size: 13px;
  color: #8a919f;
}
.score {
  font-weight: 600;
}
.avatar {
  background: #2f6fed;
  color: #fff;
}
/*卡片*/
.card-col {
  margin-bottom: 16px;
}
.stu-card__top {
  display: flex;
  gap: 12px;
  align-items: center;
}
.stu-card__name {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
  color: #1f2329;
}
.stu-card__id {
  margin: 4px 0 0;
  font-size: 12px;
  color: #8a919f;
}
.stu-card__tags {
  display: flex;
  gap: 8px;
  margin: 14px 0;
}
.stu-card__score {
  margin: 0 0 6px;
  font-size: 13px;
  color: #5c6470;
}
/*名单视图*/
.name-list {
  margin: 0;
  padding: 0;
  list-style: none;
  border: 1px solid #ebeef5;
  border-radius: 8px;
  overflow: hidden;
}
.name-list__item {
  display: flex;
  gap: 12px;
  align-items: center;
  padding: 10px 16px;
  background: #fff;
  border-bottom: 1px solid #f2f3f5;
}
.name-list__item:last-child {
  border-bottom: none;
}
.name-list__item:hover {
  background: #f7f9fc;
}
.name-list__name {
  width: 80px;
  font-weight: 600;
  color: #1f2329;
}
.name-list__meta {
  flex: 1;
  font-size: 13px;
  color: #8a919f;
}

/*分组视图*/
.group-view {
  display: flex;
  flex-direction: column;
  gap: 24px;
}
.group-block {
  border:1px solid #ebeef5;
  border-radius:8px;
  overflow:hidden;
}
.group-title {
  margin:0;
  padding:10px 16px;
  background:#f2f3f5;
  font-size:16px;
}
.group-list {
  margin:0;
  padding:0;
  list-style:none;
}
.group-item {
  display:flex;
  gap:12px;
  align-items:center;
  padding:10px 16px;
  border-bottom:1px solid #f2f3f5;
}
.group-item:last-child {
  border-bottom:none;
}
.group-item:hover{
  background:#f7f9fc;
}
.g-name{
  width:80px;
  font-weight:600;
}
.g-meta{
  flex:1;
  font-size:13px;
  color:#8a919f;
}
/*弹窗详情样式*/
.detail-box p {
  font-size:15px;
  line-height:2;
  margin:4px 0;
}
</style>