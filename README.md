# frontend‑learning

> 前端工程化学习练习仓库，用于练习 Git、项目工程化基础配置。

## 📋 仓库简介

本仓库用于学习前端工程化相关知识，练习 Git 版本控制、项目忽略配置、代码提交推送流程。

主要练习内容：Git 仓库初始化、文件暂存提交、远程仓库关联推送、`.gitignore`忽略文件配置。

## 📂 目录说明

plaintext





```
frontend‑learning/
├── .gitignore          # Git忽略配置文件
└── README.md           # 项目说明文档
```

## ✨ 已完成操作

1. 使用 `git init` 初始化本地 Git 仓库
2. 编写 `.gitignore`，配置项目不需要提交的文件 / 目录过滤规则
3. 创建项目说明文档 `README.md`
4. `git add` 将文件加入暂存区
5. `git commit` 完成本地版本提交
6. 在 GitHub 创建远程空仓库
7. `git remote add` 关联远程仓库地址
8. `git push` 将本地版本推送到 GitHub 远程仓库

## 📝 Git 常用命令记录

bash





```
# 初始化本地仓库
git init

# 查看仓库状态
git status

# 添加文件到暂存区
git add 文件名
git add .

# 提交版本，填写提交信息
git commit -m "提交描述"

# 关联远程仓库
git remote add origin 仓库地址

# 推送本地分支到远程
git push -u origin main

# 查看提交日志
git log
```

## 🚀 后续计划

- 学习 npm /yarn 包管理
- 学习 Vite 项目搭建
- 学习 ESLint、Prettier 代码规范配置
- 学习打包构建流程

## 📄 许可证

MIT

------

### 使用方法

把上面全部内容复制，粘贴到你的 `README.md` 文件，保存。

然后执行命令提交更新：

bash





```
git add README.md
git commit -m "docs: 更新仓库README文档"
git push
```

执行完成后刷新 GitHub 网页，README 就会展示在仓库首页。