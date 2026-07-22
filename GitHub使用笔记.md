# GitHub 使用笔记

> 练习仓库：[fruit-practice](https://github.com/liwankuanglan-X/fruit-practice)
> 笔记日期：2026-07-22

---

## 一、三个工具，各管什么

| 工具 | 在哪用 | 干什么 | 关系 |
|------|--------|--------|------|
| **VS Code 内置 Git** | 左侧源代码管理 `Ctrl+Shift+G` | 暂存、提交、推送、拉取 | 日常最常用 |
| **GitHub Pull Requests 插件** | 左侧 GitHub 图标 | 创建/查看/审查 PR | 团队协作必备 |
| **gh CLI** | 终端 `Ctrl+`` | 所有 GitHub 操作的命令行版 | 备用，敲命令更快 |

**注意**：VS Code 插件的登录和 gh CLI 的登录是两套独立的，各登各的。

---

## 二、完整工作流程（从写代码到合并）

### 2.1 日常开发（小改动，直接推主分支）

```
1. 改代码
2. Ctrl+S 保存
3. Ctrl+Shift+G 打开源代码管理
4. 点文件旁边的 + 号（暂存）
5. 输入提交消息（例如："修复了登录bug"）
6. Ctrl+Enter 提交
7. 点底部状态栏 ↻ 同步更改（推送到 GitHub）
```

### 2.2 团队开发（新功能，走分支+PR）

```
1. 切到 master 分支（点左下角分支名 → 选 master）
2. 同步最新代码（点底部 ↻ 同步更改）
3. Ctrl+Shift+P → "创建分支" → 起个名字（如 feature/xxx）
4. 在新分支上改代码
5. 暂存 → 提交 → 推送（和 2.1 的 3-7 步一样）
6. Ctrl+Shift+P → "Create Pull Request" → 填写标题 → Create
7. 别人审查通过后，在 GitHub 网页点 "Merge pull request"
8. 切回 master → 点 ↻ 同步拉取最新代码
```

---

## 三、分支到底是什么

```
master（主干/正式版）         feature/add-apple（树枝/草稿）
     │                              │
     ├── 🍌 香蕉                    ├── 🍌 香蕉
     │                              ├── 🍎 苹果（新功能）
     │                              │
     │         ←──── PR 合并 ────→   │
     ▼                              ▼
  代码合进来了                  任务完成，分支可删
```

- **master**：正式代码，不能乱改
- **feature 分支**：你写新功能的草稿，改坏了也不影响 master
- **PR（Pull Request）**：把草稿提交上去，申请合并到 master

---

## 四、回退代码

### 方式 A：`git revert`（推荐，安全）

```bash
# 回退最近一次提交，保留所有历史记录
git revert HEAD -m 1 --no-edit
git push
```

- 原理：创建一个"反向提交"来抵消之前的改动
- 适合：已经推送到 GitHub 的代码

### 方式 B：`git reset`（危险，仅限本地）

```bash
# 回到上一个版本，历史被抹掉
git reset --hard HEAD~1

# 回到指定版本
git reset --hard <提交ID>

# 查看提交ID
git log --oneline
```

- 原理：直接让 HEAD 指回过去的某个版本
- 适合：还没推送的本地提交

---

## 五、常用命令速查（终端）

| 命令 | 作用 |
|------|------|
| `git status` | 查看当前状态 |
| `git log --oneline` | 查看提交历史 |
| `git branch` | 查看所有本地分支 |
| `git checkout 分支名` | 切换分支 |
| `git checkout -b 分支名` | 创建并切换分支 |
| `git pull` | 拉取最新代码 |
| `git push` | 推送代码 |
| `git revert HEAD -m 1 --no-edit` | 回退最近一次合并 |
| `git reset --hard HEAD~1` | 暴力回退一个版本 |

---

## 六、常见问题

### Q：为什么 `git push` 报错 "Connection was reset"？
**A**：国内访问 GitHub 需要梯子。换个节点再试。

### Q：为什么 `git revert` 报错 "is a merge but no -m option was given"？
**A**：因为你回退的是合并提交，需要加 `-m 1` 告诉 Git 以哪个分支为主线。

### Q：gh CLI 需要一直开着吗？
**A**：不需要。gh 就是一条命令，用的时候敲一下就行，不是后台服务。

### Q：VS Code 插件登录了，为什么 gh CLI 还要登录？
**A**：两套独立的认证系统，各登各的。

### Q：左下角显示的是什么？
**A**：当前分支名。点它可以切换分支或创建新分支。

---

## 七、本次练习记录

```
完整流程：
  🍌 初始代码（master）
    ├── 直接在 master 上加了 🍎 苹果 → 提交推送
    ├── 创建 feature/add-grape 分支 → 加 🍇 葡萄 → 提 PR
    ├── 在 GitHub 网页合并 PR
    └── 用 git revert 回退了葡萄，回到 🍌+🍎
```
