# javaweb 学习工程

黑马程序员 JavaWeb 课程（day01–day15）多模块聚合工程，供 IDEA 对照学习。

## 结构
- `day04`~`day15` 各 day 目录下是课程自带 Maven/SpringBoot 子工程（解压整理，保留原包名 com.itheima）
- `frontend/` 前端 day01-03（HTML/JS/Vue/Element，非 Maven）
- `day06`/`day07` 纯 SQL 脚本
- `tlias-final/` 员工管理项目最终版（day15 多模块版，含 CRUD+上传+JWT+AOP）
- 每个 day 目录 `notes/` 拷对应讲义 md

## 编译
命令行需 JAVA_HOME 指向 JDK 21（机器默认 Maven 跑 JDK 8 会失败）：
`JAVA_HOME=D:/java/jre21.0.12 mvn validate`

部分 MyBatis 工程运行需 MySQL 建库（见各 day 的 SQL 脚本/资料），本工程只保证编译通过。
