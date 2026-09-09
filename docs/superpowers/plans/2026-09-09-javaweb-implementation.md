# javaweb 多模块聚合工程 实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 把 B站黑马程序员 JavaWeb 课程（day01–day15）自带的 Maven/SpringBoot 工程与前端/SQL 资料，整理成一个可对照学习的 IDEA + Maven 多模块聚合工程，落到 `D:\javaproject\javaweb`，每个 day 目录带 `notes/` 拷对应讲义 md。

**Architecture:** Maven 聚合器（根 `pom.xml` packaging=pom + `<modules>`）下挂各 day 的课程子工程（保留原 pom、原包名 `com.itheima`）；前端 day01-03 与纯 SQL day06-07 作为普通目录（非 Maven 模块）；tlias 只取 day15 多模块最终版一份；zip 全部解压并清掉 `target/`/`*.iml`。

**Tech Stack:** JDK 21（命令行 `mvn` 需 `JAVA_HOME=D:/java/jre21.0.12`），Maven 3.9.16，SpringBoot 2.7.4（课程 pom 自带），Java 11 target，MyBatis，前端原生 HTML/JS/Vue2/Element。

## Global Constraints

- 工程根：`D:\javaproject\javaweb`。源资料根：`D:\study\JavaWeb`。
- 命令行 `mvn` 编译必须 `JAVA_HOME=D:/java/jre21.0.12`（机器默认 Maven runtime=JDK8 编不过 Java 11 target）。验证命令统一写成 `JAVA_HOME="D:/java/jre21.0.12" mvn ...`。
- 包名**保留 `com.itheima`**（课程原包名），不改名、不改业务代码（外科手术式）。
- zip 解压后剔除：`target/`、`*.iml`、`.idea/`（如有）。保留 `src`+`pom.xml`+`src/main/resources`（含 element-ui/js 等）。`mvnw`/`mvnw.cmd`/`.mvn/` 一并剔除（保持干净，子 pom 不依赖 wrapper 运行）。
- md 笔记拷到对应目录的 `notes/`，**连同 `讲义/assets/` 图片目录一起拷**，保证 IDEA markdown 预览显示图片。md 原样不改。
- 聚合器根 `pom.xml` 的 `<modules>` 只列有 pom 的 Maven 子工程；前端、纯 SQL 目录不列入。
- day15 的 `02.继承与聚合` 和 `03.私服操作` 含 `tlias-parent`+3 子模块，与 `tlias-final` 同名同包 → **不进根聚合器**，作为独立 reactor 各自含父 pom。
- tlias 取 day15 `01. 多模块开发` 版（最终版，含 CRUD+上传+JWT+AOP），不重复拷 day10/11/12/13 的 tlias zip。
- 每个 Task 结束 `git add -A && git commit`（按 task 提交，同 javastudy）。git user 用仓库已有 `gy`。
- 不建库建表、不写额外测试、不为前端引入 Node 构建。

## File Structure

工程骨架（聚合器 + 各 day 子目录）：
```
D:\javaproject\javaweb\
├── pom.xml                      # 聚合器 (Task 0 建, 后续 task 逐步加 <modules>)
├── README.md
├── .gitignore                   # 已随 spec 提交, Task 0 确认
├── docs/                        # spec + plan (已存在)
├── frontend/                    # Task 9 (day01-03, 非 Maven)
├── day04-maven-springboot/      # Task 1 (7 子工程)
├── day05-req-resp/              # Task 2 (4 版)
├── day06-mysql/                 # Task 3 (纯 SQL)
├── day07-mysql/                 # Task 3 (纯 SQL)
├── day08-mybatis/               # Task 4
├── day09-mybatis/               # Task 4
├── tlias-final/                 # Task 5 (day15/01, 3 模块)
├── day13-aop/                   # Task 6
├── day14-springboot-principle/  # Task 7 (3 组)
└── day15-maven-advanced/        # Task 8 (02/03, 独立 reactor)
```

各 Task 的「Files」「Interfaces」块见下方。

---

### Task 0: 聚合器骨架

**Files:**
- Create: `D:\javaproject\javaweb\pom.xml`
- Create: `D:\javaproject\javaweb\README.md`
- Verify: `.gitignore`（已随 spec 提交，应已存在）

**Interfaces:**
- Consumes: 无
- Produces: 合法的聚合器 `pom.xml`（空 `<modules>`，后续 Task 逐步追加）；可跑 `mvn validate` 的工程根

- [ ] **Step 1: 写聚合器 pom.xml（空 modules）**

写入 `D:\javaproject\javaweb\pom.xml`：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.itheima</groupId>
    <artifactId>javaweb-aggregator</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>javaweb-aggregator</name>
    <description>黑马 JavaWeb 课程多模块聚合工程（对照学习用）</description>
    <modules>
        <!-- 后续 Task 逐个追加 Maven 子工程 -->
    </modules>
</project>
```

- [ ] **Step 2: 写 README.md**

写入 `D:\javaproject\javaweb\README.md`：

```markdown
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
```

- [ ] **Step 3: 验证聚合器合法**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate && echo "POM_OK"
```
Expected: `POM_OK`

- [ ] **Step 4: 提交**

```bash
cd /d/javaproject/javaweb
git add pom.xml README.md
git commit -m "Task 0: 聚合器骨架 pom.xml + README

Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Task 1: day04 Maven & SpringBoot 入门（7 子工程）

**Files:**
- Create: `day04-maven-springboot/` 下 7 个子工程目录（从 `D:\study\JavaWeb\day04-Maven-SpringBootWeb入门\代码\` 拷贝/移动）
  - `maven-project01`, `maven-project02`, `maven-projectA`, `maven-projectB`, `maven-projectC`, `http-server-demo`, `springboot-web-quickstart`
- Create: `day04-maven-springboot/notes/` 拷 2 份 md + assets
- Modify: `pom.xml`（聚合器加 7 个 module）

**Interfaces:**
- Consumes: Task 0 聚合器
- Produces: 7 个可被聚合器识别的 Maven 子模块；`mvn validate` 通过

- [ ] **Step 1: 创建目录 + 拷贝 7 个子工程（原样，未压 zip 直接 cp -r）**

```bash
cd /d/javaproject/javaweb
mkdir -p day04-maven-springboot
SRC="/d/study/JavaWeb/day04-Maven-SpringBootWeb入门/代码"
for p in maven-project01 maven-project02 maven-projectA maven-projectB maven-projectC http-server-demo springboot-web-quickstart; do
  cp -r "$SRC/$p" "day04-maven-springboot/$p"
done
```

- [ ] **Step 2: 清理每个子工程的 target/ 与 *.iml**

```bash
cd /d/javaproject/javaweb/day04-maven-springboot
find . -type d -name target -exec rm -rf {} + 2>/dev/null
find . -name "*.iml" -delete 2>/dev/null
find . -name ".idea" -type d -exec rm -rf {} + 2>/dev/null
# 剔除 maven wrapper（子 pom 不依赖）
find . -name "mvnw" -delete 2>/dev/null
find . -name "mvnw.cmd" -delete 2>/dev/null
find . -name ".mvn" -type d -exec rm -rf {} + 2>/dev/null
echo "cleaned"
find . -maxdepth 2 -name pom.xml | wc -l   # 期望 7
```
Expected: 输出 `cleaned`，pom 数 = 7

- [ ] **Step 3: 拷 notes（2 份 md + assets）**

```bash
cd /d/javaproject/javaweb/day04-maven-springboot
mkdir -p notes
SRC="/d/study/JavaWeb/day04-Maven-SpringBootWeb入门/讲义"
cp "$SRC/01. Maven/Maven.md" notes/
cp -r "$SRC/01. Maven/assets" notes/ 2>/dev/null
cp "$SRC/02. SpringBootWeb入门/SpringBootWeb入门.md" notes/
cp -r "$SRC/02. SpringBootWeb入门/assets" notes/assets-springbootweb 2>/dev/null  # 避免与上一个 assets 重名
ls notes/
```
Expected: `notes/` 下有 `Maven.md`、`SpringBootWeb入门.md` 及 assets 目录

- [ ] **Step 4: 聚合器加 7 个 module**

编辑 `D:\javaproject\javaweb\pom.xml`，把空 `<modules>` 替换为：

```xml
    <modules>
        <module>day04-maven-springboot/maven-project01</module>
        <module>day04-maven-springboot/maven-project02</module>
        <module>day04-maven-springboot/maven-projectA</module>
        <module>day04-maven-springboot/maven-projectB</module>
        <module>day04-maven-springboot/maven-projectC</module>
        <module>day04-maven-springboot/http-server-demo</module>
        <module>day04-maven-springboot/springboot-web-quickstart</module>
    </modules>
```

- [ ] **Step 5: 验证聚合器识别 + 编译**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate && echo "VALIDATE_OK"
JAVA_HOME="D:/java/jre21.0.12" mvn -q -pl day04-maven-springboot/springboot-web-quickstart compile && echo "COMPILE_OK_QUICKSTART"
```
Expected: `VALIDATE_OK`，且 quickstart `COMPILE_OK_QUICKSTART`（需联网拉 SpringBoot 依赖）

> 若 `maven-projectA/B/C` 因相互依赖或依赖本地仓库缺包而 compile 失败，记下失败模块，本 Task 只保证 `validate` 通过 + quickstart 编译通过即可放行（maven-project* 是 Maven 入门碎工程，依赖可能不全）。在提交信息里注明哪些未通过编译。

- [ ] **Step 6: 提交**

```bash
cd /d/javaproject/javaweb
git add -A
git commit -m "Task 1: day04 Maven&SpringBoot入门 (7子工程 + notes)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Task 2: day05 请求响应（4 迭代版本）

**Files:**
- Create: `day05-req-resp/01-统一响应/springboot-web-req-resp`（解压 zip #1）
- Create: `day05-req-resp/02-三层架构拆分/springboot-web-req-resp`
- Create: `day05-req-resp/03-IOC入门/springboot-web-req-resp`
- Create: `day05-req-resp/04-IOC详解/springboot-web-req-resp`
- Create: `day05-req-resp/notes/`（SpringBootWeb请求响应.md + assets）
- Modify: `pom.xml`（加 4 module）

**Interfaces:**
- Consumes: Task 0 聚合器
- Produces: 4 个同名（`springboot-web-req-resp` artifactId）但落不同父目录的 Maven 子模块；`mvn validate` 通过

> 注意：4 个 zip 解压后顶层都叫 `springboot-web-req-resp`，落在各自带编号父目录下，artifactId 相同但路径不同，Maven 以目录路径区分，聚合器 `<module>` 路径唯一即可。但**同一聚合器下同名 artifactId 的模块会触发 Maven "duplicate module" 警告**——实际验证时若 `mvn validate` 报重复，则只把第 4 版（IOC详解，最完整）进聚合器，前 3 版作为普通目录不进 `<modules>`（在 Step 5 处理）。

- [ ] **Step 1: 创建 4 个父目录 + 解压 4 个 zip**

```bash
cd /d/javaproject/javaweb
mkdir -p day05-req-resp/{01-统一响应,02-三层架构拆分,03-IOC入门,04-IOC详解}
SRC="/d/study/JavaWeb/day05-SpringBootWeb请求响应/代码"
# 每个目录下一个 zip, 顶层解压出 springboot-web-req-resp/
(cd "day05-req-resp/01-统一响应" && unzip -q "$SRC/01. 统一响应结果/springboot-web-req-resp.zip")
(cd "day05-req-resp/02-三层架构拆分" && unzip -q "$SRC/02. 三层架构拆分/springboot-web-req-resp.zip")
(cd "day05-req-resp/03-IOC入门" && unzip -q "$SRC/03. IOC&DI入门/springboot-web-req-resp.zip")
(cd "day05-req-resp/04-IOC详解" && unzip -q "$SRC/04. IOC&DI详解/springboot-web-req-resp.zip")
find day05-req-resp -name pom.xml | wc -l   # 期望 4
```
Expected: pom 数 = 4

- [ ] **Step 2: 清理 target/*.iml/wrapper**

```bash
cd /d/javaproject/javaweb/day05-req-resp
find . -type d -name target -exec rm -rf {} + 2>/dev/null
find . -name "*.iml" -delete 2>/dev/null
find . -name ".idea" -type d -exec rm -rf {} + 2>/dev/null
find . -name "mvnw" -o -name "mvnw.cmd" | xargs -r rm -f 2>/dev/null
find . -name ".mvn" -type d -exec rm -rf {} + 2>/dev/null
echo "cleaned"
```

- [ ] **Step 3: 拷 notes**

```bash
cd /d/javaproject/javaweb/day05-req-resp
mkdir -p notes
SRC="/d/study/JavaWeb/day05-SpringBootWeb请求响应/讲义"
cp "$SRC/SpringBootWeb请求响应.md" notes/
cp -r "$SRC/assets" notes/
ls notes/
```

- [ ] **Step 4: 聚合器加 module（先全加试 validate）**

编辑 `pom.xml`，在现有 `<modules>` 末尾追加：

```xml
        <module>day05-req-resp/01-统一响应/springboot-web-req-resp</module>
        <module>day05-req-resp/02-三层架构拆分/springboot-web-req-resp</module>
        <module>day05-req-resp/03-IOC入门/springboot-web-req-resp</module>
        <module>day05-req-resp/04-IOC详解/springboot-web-req-resp</module>
```

- [ ] **Step 5: 验证；若报 duplicate 则只留第 4 版进 modules**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate 2>&1 | tee /tmp/d05validate.log
grep -qi "duplicate" /tmp/d05validate.log && echo "DUP_DETECTED" || echo "VALIDATE_OK"
```

若 `DUP_DETECTED`：编辑 `pom.xml`，**删除** 01/02/03 三行 module（只保留 04-IOC详解 那行），01/02/03 作为普通目录保留（代码在、但不进聚合器）。再跑：
```bash
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate && echo "VALIDATE_OK_AFTER_FIX"
```
Expected: `VALIDATE_OK` 或 `VALIDATE_OK_AFTER_FIX`

- [ ] **Step 6: 编译第 4 版**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q -pl day05-req-resp/04-IOC详解/springboot-web-req-resp compile && echo "COMPILE_OK"
```
Expected: `COMPILE_OK`（需联网依赖）

- [ ] **Step 7: 提交**

```bash
cd /d/javaproject/javaweb
git add -A
git commit -m "Task 2: day05 请求响应 (4迭代版 + notes)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Task 3: day06/day07 纯 SQL（非 Maven）

**Files:**
- Create: `day06-mysql/notes/`（数据库-MySQL-01.md + assets + .sql 脚本）
- Create: `day07-mysql/notes/`（数据库-MySQL-02.md + assets + .sql/.txt）

**Interfaces:**
- Consumes: 无（不进聚合器）
- Produces: 两个纯资料目录，无 pom，浏览器/IDEA 可读

- [ ] **Step 1: day06 拷 notes + SQL 脚本**

```bash
cd /d/javaproject/javaweb
mkdir -p day06-mysql/notes
SRC="/d/study/JavaWeb/day06-MySQL"
cp "$SRC/讲义/数据库-MySQL-01.md" day06-mysql/notes/
cp -r "$SRC/讲义/assets" day06-mysql/notes/
cp "$SRC/代码/01. DQL上课演示脚本.txt" day06-mysql/
ls day06-mysql/ day06-mysql/notes/
```
Expected: `day06-mysql/` 下有 `01. DQL上课演示脚本.txt`，`notes/` 下有 md + assets

- [ ] **Step 2: day07 拷 notes + SQL 脚本**

```bash
cd /d/javaproject/javaweb
mkdir -p day07-mysql/notes
SRC="/d/study/JavaWeb/day07-MySQL"
cp "$SRC/讲义/数据库-MySQL-02.md" day07-mysql/notes/
cp -r "$SRC/讲义/assets" day07-mysql/notes/
cp "$SRC/代码/01. DQL上课演示脚本.sql" day07-mysql/ 2>/dev/null
cp "$SRC/代码/02. 多表设计.txt" day07-mysql/ 2>/dev/null
ls day07-mysql/ day07-mysql/notes/
```

- [ ] **Step 3: 验证聚合器仍合法（这两个不进 modules）**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate && echo "VALIDATE_OK"
```
Expected: `VALIDATE_OK`

- [ ] **Step 4: 提交**

```bash
cd /d/javaproject/javaweb
git add -A
git commit -m "Task 3: day06/day07 纯SQL资料 (notes + 脚本)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Task 4: day08/day09 MyBatis

**Files:**
- Create: `day08-mybatis/springboot-mybatis-quickstart`（解压「完整代码」zip）
- Create: `day08-mybatis/notes/`（Mybatis入门.md + 数据库-MySQL-03.md + assets）
- Create: `day09-mybatis/springboot-mybatis-crud`（解压 zip）
- Create: `day09-mybatis/notes/`（Mybatis.md + assets + SQL脚本.txt）
- Modify: `pom.xml`（加 2 module）

**Interfaces:**
- Consumes: Task 0 聚合器
- Produces: 2 个 MyBatis Maven 子模块；`mvn compile` 通过

- [ ] **Step 1: day08 解压「完整代码」版（非入门程序骨架版）**

```bash
cd /d/javaproject/javaweb
mkdir -p day08-mybatis
SRC="/d/study/JavaWeb/day08-MySQL-Mybatis入门/代码"
(cd day08-mybatis && unzip -q "$SRC/mybatis完整代码/springboot-mybatis-quickstart.zip")
# 解压出 springboot-mybatis-quickstart/，移到 day08-mybatis/ 下
ls day08-mybatis/springboot-mybatis-quickstart/pom.xml   # 应存在
```
Expected: 该 pom 路径存在

- [ ] **Step 2: day08 清理 + notes**

```bash
cd /d/javaproject/javaweb/day08-mybatis
find . -type d -name target -exec rm -rf {} + 2>/dev/null
find . -name "*.iml" -delete 2>/dev/null
find . -name ".idea" -type d -exec rm -rf {} + 2>/dev/null
mkdir -p notes
SRC="/d/study/JavaWeb/day08-MySQL-Mybatis入门/讲义"
cp "$SRC/02-Mybatis入门/Mybatis入门.md" notes/
cp -r "$SRC/02-Mybatis入门/assets" notes/ 2>/dev/null
cp "$SRC/01-MySQL/数据库-MySQL-03.md" notes/
cp -r "$SRC/01-MySQL/assets" notes/assets-mysql03 2>/dev/null
ls notes/
```

- [ ] **Step 3: day09 解压 + 清理 + notes**

```bash
cd /d/javaproject/javaweb
mkdir -p day09-mybatis
SRC="/d/study/JavaWeb/day09-Mybatis/代码"
(cd day09-mybatis && unzip -q "$SRC/springboot-mybatis-crud.zip")
cd day09-mybatis
find . -type d -name target -exec rm -rf {} + 2>/dev/null
find . -name "*.iml" -delete 2>/dev/null
find . -name ".idea" -type d -exec rm -rf {} + 2>/dev/null
mkdir -p notes
cp "/d/study/JavaWeb/day09-Mybatis/讲义/Mybatis.md" notes/
cp -r "/d/study/JavaWeb/day09-Mybatis/讲义/assets" notes/ 2>/dev/null
cp "/d/study/JavaWeb/day09-Mybatis/代码/SQL脚本.txt" . 2>/dev/null
ls springboot-mybatis-crud/pom.xml notes/
```
Expected: pom 与 notes 都在

- [ ] **Step 4: 聚合器加 2 module**

编辑 `pom.xml`，在 `<modules>` 末尾追加：

```xml
        <module>day08-mybatis/springboot-mybatis-quickstart</module>
        <module>day09-mybatis/springboot-mybatis-crud</module>
```

- [ ] **Step 5: 验证 + 编译**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate && echo "VALIDATE_OK"
JAVA_HOME="D:/java/jre21.0.12" mvn -q -pl day08-mybatis/springboot-mybatis-quickstart,day09-mybatis/springboot-mybatis-crud compile && echo "COMPILE_OK"
```
Expected: `VALIDATE_OK`、`COMPILE_OK`（需联网拉 MyBatis + mysql-connector）

- [ ] **Step 6: 提交**

```bash
cd /d/javaproject/javaweb
git add -A
git commit -m "Task 4: day08/day09 MyBatis (quickstart完整版 + crud + notes)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Task 5: tlias-final（day15/01 多模块最终版）

**Files:**
- Create: `tlias-final/tlias-pojo`（解压）
- Create: `tlias-final/tlias-utils`（解压）
- Create: `tlias-final/tlias-web-management`（解压）
- Create: `tlias-final/notes/`（4 份讲义 md + 各自 assets）
- Modify: `pom.xml`（加 3 module）

**Interfaces:**
- Consumes: Task 0 聚合器
- Produces: tlias 3 模块 reactor；`mvn compile` 通过（web-management 依赖 pojo/utils，reactor 自动排序）

> day15 `01. 多模块开发.zip` 内层是 `01. 多模块开发/tlias-pojo|tlias-utils|tlias-web-management/`。解压后把三个 tlias-* 目录提到 `tlias-final/` 顶层，丢弃 `01. 多模块开发/` 这层中文目录。

- [ ] **Step 1: 解压到 tlias-final，提平三个模块**

```bash
cd /d/javaproject/javaweb
mkdir -p tlias-final
SRC="/d/study/JavaWeb/day15-maven高级/代码/01. 多模块开发.zip"
# 临时解压到 tmp，再把内层三个 tlias-* 移到 tlias-final/
TMP=$(mktemp -d)
(cd "$TMP" && unzip -q "$SRC")   # 解出 "01. 多模块开发/" 目录
mv "$TMP/01. 多模块开发/tlias-pojo" tlias-final/
mv "$TMP/01. 多模块开发/tlias-utils" tlias-final/
mv "$TMP/01. 多模块开发/tlias-web-management" tlias-final/
rm -rf "$TMP"
ls tlias-final/   # 期望: tlias-pojo  tlias-utils  tlias-web-management
find tlias-final -name pom.xml | wc -l   # 期望 3
```
Expected: 3 个目录，3 个 pom

- [ ] **Step 2: 清理**

```bash
cd /d/javaproject/javaweb/tlias-final
find . -type d -name target -exec rm -rf {} + 2>/dev/null
find . -name "*.iml" -delete 2>/dev/null
find . -name ".idea" -type d -exec rm -rf {} + 2>/dev/null
echo cleaned
```

- [ ] **Step 3: 拷 4 份讲义到 notes（记演进）**

```bash
cd /d/javaproject/javaweb/tlias-final
mkdir -p notes
cp "/d/study/JavaWeb/day10-SpringBootWeb案例/讲义/SpringBootWeb案例-1.md" notes/
cp -r "/d/study/JavaWeb/day10-SpringBootWeb案例/讲义/assets" notes/assets-day10 2>/dev/null
cp "/d/study/JavaWeb/day11-SpringBootWeb案例/讲义/SpringBootWeb案例-2.md" notes/
cp -r "/d/study/JavaWeb/day11-SpringBootWeb案例/讲义/assets" notes/assets-day11 2>/dev/null
cp "/d/study/JavaWeb/day12-SpringBootWeb登录认证/讲义/SpringBootWeb登录认证.md" notes/
cp -r "/d/study/JavaWeb/day12-SpringBootWeb登录认证/讲义/assets" notes/assets-day12 2>/dev/null
cp "/d/study/JavaWeb/day13-SpringBootWeb AOP/讲义/SpringBootWeb AOP.md" notes/
cp -r "/d/study/JavaWeb/day13-SpringBootWeb AOP/讲义/assets" notes/assets-day13 2>/dev/null
ls notes/
```

- [ ] **Step 4: 聚合器加 3 module**

编辑 `pom.xml`，在 `<modules>` 末尾追加：

```xml
        <module>tlias-final/tlias-pojo</module>
        <module>tlias-final/tlias-utils</module>
        <module>tlias-final/tlias-web-management</module>
```

- [ ] **Step 5: 验证 + reactor 编译**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate && echo "VALIDATE_OK"
JAVA_HOME="D:/java/jre21.0.12" mvn -q -pl tlias-final/tlias-pojo,tlias-final/tlias-utils,tlias-final/tlias-web-management compile && echo "COMPILE_OK"
```
Expected: `VALIDATE_OK`、`COMPILE_OK`（web-management 依赖 pojo/utils，reactor 自动排序编译）

- [ ] **Step 6: 提交**

```bash
cd /d/javaproject/javaweb
git add -A
git commit -m "Task 5: tlias-final 最终版 (day15多模块: pojo+utils+web-management + 4份演进讲义)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Task 6: day13 AOP 独立教学工程

**Files:**
- Create: `day13-aop/springboot-aop-quickstart`（解压）
- Create: `day13-aop/notes/`（SpringBootWeb AOP.md + assets）
- Modify: `pom.xml`（加 1 module）

**Interfaces:**
- Consumes: Task 0 聚合器
- Produces: AOP 教学 Maven 子模块；`mvn compile` 通过

- [ ] **Step 1: 解压 + 清理**

```bash
cd /d/javaproject/javaweb
mkdir -p day13-aop
SRC="/d/study/JavaWeb/day13-SpringBootWeb AOP/代码/02. AOP/springboot-aop-quickstart.zip"
(cd day13-aop && unzip -q "$SRC")   # 解出 springboot-aop-quickstart/
cd day13-aop
find . -type d -name target -exec rm -rf {} + 2>/dev/null
find . -name "*.iml" -delete 2>/dev/null
find . -name ".idea" -type d -exec rm -rf {} + 2>/dev/null
ls springboot-aop-quickstart/pom.xml
```

- [ ] **Step 2: 拷 notes**

```bash
cd /d/javaproject/javaweb/day13-aop
mkdir -p notes
cp "/d/study/JavaWeb/day13-SpringBootWeb AOP/讲义/SpringBootWeb AOP.md" notes/
cp -r "/d/study/JavaWeb/day13-SpringBootWeb AOP/讲义/assets" notes/ 2>/dev/null
ls notes/
```

- [ ] **Step 3: 聚合器加 module + 验证编译**

编辑 `pom.xml`，追加：

```xml
        <module>day13-aop/springboot-aop-quickstart</module>
```

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate && echo "VALIDATE_OK"
JAVA_HOME="D:/java/jre21.0.12" mvn -q -pl day13-aop/springboot-aop-quickstart compile && echo "COMPILE_OK"
```
Expected: `VALIDATE_OK`、`COMPILE_OK`

- [ ] **Step 4: 提交**

```bash
cd /d/javaproject/javaweb
git add -A
git commit -m "Task 6: day13 AOP 教学工程 (springboot-aop-quickstart + notes)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Task 7: day14 SpringBoot 原理篇（3 组共 4 模块）

**Files:**
- Create: `day14-springboot-principle/01-配置优先级/springboot-web-config`（解压）
- Create: `day14-springboot-principle/02-bean管理/springboot-web-config2`（解压）
- Create: `day14-springboot-principle/03-自定义starter/aliyun-oss-spring-boot-autoconfigure`（解压）
- Create: `day14-springboot-principle/03-自定义starter/aliyun-oss-spring-boot-starter`（解压）
- Create: `day14-springboot-principle/notes/`（SpringBoot原理篇.md + assets）
- Modify: `pom.xml`（加 4 module）

**Interfaces:**
- Consumes: Task 0 聚合器
- Produces: 4 个原理演示 Maven 子模块；`mvn compile` 通过

- [ ] **Step 1: 创建目录 + 解压 4 个 zip**

```bash
cd /d/javaproject/javaweb
mkdir -p day14-springboot-principle/{01-配置优先级,02-bean管理,03-自定义starter}
SRC="/d/study/JavaWeb/day14-SpringBoot原理篇/代码"
(cd "day14-springboot-principle/01-配置优先级" && unzip -q "$SRC/01. 配置优先级/springboot-web-config.zip")
(cd "day14-springboot-principle/02-bean管理" && unzip -q "$SRC/02. bean的管理/springboot-web-config2.zip")
(cd "day14-springboot-principle/03-自定义starter" && unzip -q "$SRC/03. 自定义starter/aliyun-oss-spring-boot-autoconfigure.zip")
(cd "day14-springboot-principle/03-自定义starter" && unzip -q "$SRC/03. 自定义starter/aliyun-oss-spring-boot-starter.zip")
find day14-springboot-principle -name pom.xml | wc -l   # 期望 4
```
Expected: pom 数 = 4

- [ ] **Step 2: 清理**

```bash
cd /d/javaproject/javaweb/day14-springboot-principle
find . -type d -name target -exec rm -rf {} + 2>/dev/null
find . -name "*.iml" -delete 2>/dev/null
find . -name ".idea" -type d -exec rm -rf {} + 2>/dev/null
echo cleaned
```

- [ ] **Step 3: 拷 notes**

```bash
cd /d/javaproject/javaweb/day14-springboot-principle
mkdir -p notes
cp "/d/study/JavaWeb/day14-SpringBoot原理篇/讲义/SpringBoot原理篇.md" notes/
cp -r "/d/study/JavaWeb/day14-SpringBoot原理篇/讲义/assets" notes/ 2>/dev/null
ls notes/
```

- [ ] **Step 4: 聚合器加 4 module**

编辑 `pom.xml`，追加：

```xml
        <module>day14-springboot-principle/01-配置优先级/springboot-web-config</module>
        <module>day14-springboot-principle/02-bean管理/springboot-web-config2</module>
        <module>day14-springboot-principle/03-自定义starter/aliyun-oss-spring-boot-autoconfigure</module>
        <module>day14-springboot-principle/03-自定义starter/aliyun-oss-spring-boot-starter</module>
```

- [ ] **Step 5: 验证 + 编译**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate && echo "VALIDATE_OK"
JAVA_HOME="D:/java/jre21.0.12" mvn -q -pl day14-springboot-principle/01-配置优先级/springboot-web-config,day14-springboot-principle/02-bean管理/springboot-web-config2 compile && echo "COMPILE_OK_CFG"
# starter 两模块: autoconfigure 编译, starter 依赖 autoconfigure(同批 reactor)
JAVA_HOME="D:/java/jre21.0.12" mvn -q -pl day14-springboot-principle/03-自定义starter/aliyun-oss-spring-boot-autoconfigure,day14-springboot-principle/03-自定义starter/aliyun-oss-spring-boot-starter compile && echo "COMPILE_OK_STARTER"
```
Expected: `VALIDATE_OK`、`COMPILE_OK_CFG`、`COMPILE_OK_STARTER`

- [ ] **Step 6: 提交**

```bash
cd /d/javaproject/javaweb
git add -A
git commit -m "Task 7: day14 SpringBoot原理篇 (config + config2 + 自定义starter 2模块 + notes)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Task 8: day15 Maven 高级（02 继承聚合 / 03 私服，独立 reactor）

**Files:**
- Create: `day15-maven-advanced/02-inheritance-aggregation/`（解压 02 zip，4 模块：tlias-parent + 3 子）
- Create: `day15-maven-advanced/03-nexus/`（解压 03 zip，4 模块）
- Create: `day15-maven-advanced/notes/`（Maven高级.md + assets）
- **不**修改聚合器 `pom.xml`（02/03 独立 reactor，避免与 tlias-final 同名冲突）

**Interfaces:**
- Consumes: 无（不进根聚合器）
- Produces: 两个独立 4 模块 reactor，各自含 `tlias-parent` 父 pom；IDEA 单独打开

- [ ] **Step 1: 解压 02 与 03，提平模块**

```bash
cd /d/javaproject/javaweb
mkdir -p day15-maven-advanced/{02-inheritance-aggregation,03-nexus}
SRC="/d/study/JavaWeb/day15-maven高级/代码"
# 02
TMP=$(mktemp -d)
(cd "$TMP" && unzip -q "$SRC/02. 继承与聚合.zip")   # 解出 "02. 继承与聚合/" 目录
mv "$TMP/02. 继承与聚合"/* day15-maven-advanced/02-inheritance-aggregation/ 2>/dev/null
rm -rf "$TMP"
# 03
TMP=$(mktemp -d)
(cd "$TMP" && unzip -q "$SRC/03. 私服操作.zip")
mv "$TMP/03. 私服操作"/* day15-maven-advanced/03-nexus/ 2>/dev/null
rm -rf "$TMP"
find day15-maven-advanced -name pom.xml | wc -l   # 期望 8
```
Expected: pom 数 = 8（02 四个 + 03 四个）

- [ ] **Step 2: 清理**

```bash
cd /d/javaproject/javaweb/day15-maven-advanced
find . -type d -name target -exec rm -rf {} + 2>/dev/null
find . -name "*.iml" -delete 2>/dev/null
find . -name ".idea" -type d -exec rm -rf {} + 2>/dev/null
echo cleaned
```

- [ ] **Step 3: 拷 notes**

```bash
cd /d/javaproject/javaweb/day15-maven-advanced
mkdir -p notes
cp "/d/study/JavaWeb/day15-maven高级/讲义/Maven高级.md" notes/
cp -r "/d/study/JavaWeb/day15-maven高级/讲义/assets" notes/ 2>/dev/null
ls notes/
```

- [ ] **Step 4: 验证聚合器不受影响（未加 module）**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate && echo "VALIDATE_OK"
# 02 独立 reactor 编译（进入其目录, 它有自己的 tlias-parent 父 pom）
cd /d/javaproject/javaweb/day15-maven-advanced/02-inheritance-aggregation
JAVA_HOME="D:/java/jre21.0.12" mvn -q compile && echo "COMPILE_OK_02" || echo "COMPILE_FAIL_02"
```
Expected: `VALIDATE_OK`；`COMPILE_OK_02` 或 `COMPILE_FAIL_02`（02 若依赖 tlias-utils 的私服/特定依赖可能失败，记入提交信息，放行）

> 03 私服版通常需配置 Nexus 仓库地址才能拉依赖，编译大概率失败属预期，不强求。

- [ ] **Step 5: 提交**

```bash
cd /d/javaproject/javaweb
git add -A
git commit -m "Task 8: day15 Maven高级 (02继承聚合 + 03私服 独立reactor + notes, 不进根聚合)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Task 9: 前端 day01-03（非 Maven）

**Files:**
- Create: `frontend/day01-html-css/notes/`（拷 day01 PDF）
- Create: `frontend/day02-js-vue/`（23 个 *.html + js/img 子目录）+ `notes/`
- Create: `frontend/day03-vue-element/`（Ajax/*.html + js/）+ `notes/`
- **不**修改聚合器 `pom.xml`

**Interfaces:**
- Consumes: 无
- Produces: 前端资料目录，浏览器可开 HTML

> day02 有「基础代码」和「完整代码」两套，内容同名（01~23 *.html），取「完整代码」一份（更完整）。day03 代码只有 Ajax 三件套。

- [ ] **Step 1: day01 拷 PDF**

```bash
cd /d/javaproject/javaweb
mkdir -p frontend/day01-html-css/notes
cp "/d/study/JavaWeb/PDF讲义/day01-HTML-CSS.pdf" frontend/day01-html-css/notes/
ls frontend/day01-html-css/notes/
```

- [ ] **Step 2: day02 拷完整代码 HTML + notes**

```bash
cd /d/javaproject/javaweb
SRC="/d/study/JavaWeb/day02-JavaScript-Vue"
mkdir -p frontend/day02-js-vue
cp -r "$SRC/代码/完整代码/JS/"* frontend/day02-js-vue/   # 01~23 *.html + js/ + img/
mkdir -p frontend/day02-js-vue/notes
cp "$SRC/讲义/day02-JavaScript-Vue.md" frontend/day02-js-vue/notes/
cp -r "$SRC/讲义/assets" frontend/day02-js-vue/notes/ 2>/dev/null
ls frontend/day02-js-vue/ | head
find frontend/day02-js-vue -name "*.html" | wc -l   # 期望 23
```
Expected: 23 个 html

- [ ] **Step 3: day03 拷 Ajax + notes**

```bash
cd /d/javaproject/javaweb
SRC="/d/study/JavaWeb/day03-Vue-Element"
mkdir -p frontend/day03-vue-element
cp -r "$SRC/代码/Ajax/"* frontend/day03-vue-element/   # 01~03 *.html + js/
mkdir -p frontend/day03-vue-element/notes
cp "$SRC/讲义/day03_Vue_Element.md" frontend/day03-vue-element/notes/
cp -r "$SRC/讲义/assets" frontend/day03-vue-element/notes/ 2>/dev/null
ls frontend/day03-vue-element/
```

- [ ] **Step 4: 验证聚合器仍合法 + 提交**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate && echo "VALIDATE_OK"
git add -A
git commit -m "Task 9: 前端 day01-03 (HTML/CSS PDF + JS/Vue 23html + Ajax + notes)

Co-Authored-By: Claude <noreply@anthropic.com>"
```
Expected: `VALIDATE_OK`

---

### Task 10: 全量验证 + README 完善

**Files:**
- Modify: `README.md`（补最终模块清单 + 编译说明）
- Verify: 全工程

**Interfaces:**
- Consumes: Task 0–9 全部
- Produces: 通过全量 `mvn validate` 的最终工程 + 完整 README

- [ ] **Step 1: 全量 validate**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q validate 2>&1 | tee /tmp/final-validate.log
grep -qi "BUILD SUCCESS\|VALIDATE" /tmp/final-validate.log && echo "FINAL_OK" || echo "FINAL_FAIL"
```
Expected: `FINAL_OK`

- [ ] **Step 2: 全量 compile（聚合器下所有 module）**

```bash
cd /d/javaproject/javaweb
JAVA_HOME="D:/java/jre21.0.12" mvn -q compile 2>&1 | tee /tmp/final-compile.log
echo "--- 失败模块(若有) ---"
grep -E "Reactor summary|BUILD FAILURE|\[ERROR\].*module" /tmp/final-compile.log | head -30
grep -q "BUILD SUCCESS" /tmp/final-compile.log && echo "ALL_COMPILE_OK" || echo "SOME_FAIL"
```
Expected: `ALL_COMPILE_OK`；若有失败模块，列在 README「已知问题」并说明原因（如需私服/DB）

- [ ] **Step 3: 在 README 追加最终模块清单 + 已知问题**

在 `README.md` 末尾追加（根据 Step 2 实际结果填写失败项）：

```markdown
## 模块清单（聚合器 <modules>）
- day04: maven-project01, maven-project02, maven-projectA/B/C, http-server-demo, springboot-web-quickstart
- day05: springboot-web-req-resp（04-IOC详解版进聚合；01-03 同名作普通目录）
- day08: springboot-mybatis-quickstart（完整代码版）
- day09: springboot-mybatis-crud
- tlias-final: tlias-pojo, tlias-utils, tlias-web-management
- day13: springboot-aop-quickstart
- day14: springboot-web-config, springboot-web-config2, aliyun-oss-spring-boot-autoconfigure/starter

## 不进聚合器的部分
- frontend/（day01-03 前端）
- day06/day07（纯 SQL）
- day15-maven-advanced/02,03（独立 reactor，与 tlias-final 同名避免冲突）

## 已知问题
（根据 Task 10 Step 2 实际填写，如：xxx 模块因缺私服依赖编译失败，属课程资料限制，非工程问题）
```

- [ ] **Step 4: 最终提交**

```bash
cd /d/javaproject/javaweb
git add -A
git commit -m "Task 10: 全量验证 + README 完善

Co-Authored-By: Claude <noreply@anthropic.com>"
git log --oneline | head -12
```

---

## 自检（Self-Review）

**1. 规格覆盖：** 对照 spec §2 模块结构 — Task 0 聚合器骨架、Task 1 day04（7工程）、Task 2 day05（4版）、Task 3 day06/07（纯SQL）、Task 4 day08/09（MyBatis）、Task 5 tlias-final（day15/01 三模块）、Task 6 day13-aop、Task 7 day14（4模块）、Task 8 day15 02/03（独立reactor）、Task 9 前端day01-03、Task 10 全量验证。每个 spec 模块都有对应 Task。✓

**2. 占位符扫描：** 无 TBD/TODO。每个 Step 都给了具体 bash 命令（cp/unzip/find 清理）和具体 pom `<modules>` 片段。day05 同名冲突、day15 02/03 私服编译失败、03 私服需 Nexus 等已标注为「预期可放行」并要求记入提交信息，非占位。✓

**3. 一致性：** 聚合器 module 路径与各 Task 创建的目录路径一致（如 `day04-maven-springboot/springboot-web-quickstart`）。day05 同名 artifactId 冲突在 Task 2 Step 5 给了「删 01-03 只留 04」的降级方案。tlias-final 三模块名与 day15/01 zip 内层一致（tlias-pojo/tlias-utils/tlias-web-management）。day15 02/03 故意不进聚合器（spec §2.2 一致）。JAVA_HOME 值 `D:/java/jre21.0.12` 全程一致。✓

---

## Execution Handoff

Plan complete and saved to `docs/superpowers/plans/2026-09-09-javaweb-implementation.md`. Two execution options:

**1. Subagent-Driven (recommended)** - 每个 Task 派一个全新 subagent 实现，Task 间我做 review，迭代快、上下文干净。

**2. Inline Execution** - 在本会话里按 executing-plans 批量执行，带 checkpoint 供 review。

选哪种？
