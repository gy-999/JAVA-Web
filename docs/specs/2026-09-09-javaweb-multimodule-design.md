# javaweb 多模块聚合工程设计稿

> 基于 B站黑马程序员 JavaWeb 课程（day01–day15）自带资料，整理为一个可对照学习的 IDEA + Maven 多模块聚合工程。
> 参照系：`D:\javaproject\javastudy`（黑马 Java SE 笔记 → 单模块 Maven 工程）。
> 工程根目录：`D:\javaproject\javaweb`

## 0. 「之前」是什么

`D:\javaproject\javastudy` 是用户的参照项目：把黑马 Java SE 课程（day01–21）笔记里的代码片段落成一个 Maven 单模块工程 `com.javastudy.<category>.<topic>`，每个知识点一个可右键运行的 `Demo`/`Test` 类（手写），每个二级包下 `notes/` 拷对应 `*.md`，JDK 21，无第三方依赖，经 spec → plan → subagent-driven 任务流构建（见 `docs/specs/`、`docs/superpowers/plans/`）。

**关键差异（影响设计）：** JavaWeb 课程**自带完整可运行的 Maven/SpringBoot 工程**（如 `springboot-web-quickstart`：SpringBoot 2.7.4、Java 11、`com.itheima`），讲义 md 也是成品（day05 一份就 1778 行）。这与 javastudy（笔记只有代码片段、需从零手写 `.java`）本质不同。

因此「照之前做」= **复刻流程与组织纪律**（spec → plan → 任务、`notes/` 对照、外科手术式不改业务代码、JDK 21、git 按任务提交），而工程形态按用户选择定为**多模块聚合**（而非 javastudy 的手写单模块）。

## 1. 关键决策

| 项 | 决策 | 理由 |
|---|---|---|
| 工程根 | `D:\javaproject\javaweb`（新建） | 与 javastudy 同级，命名对齐 |
| 形态 | **Maven 聚合器**：根 `pom.xml` `packaging=pom` + `<modules>`；各子工程**保留原 pom**（SpringBoot 子模块继续继承 `spring-boot-starter-parent`） | 聚合器 ≠ 父工程，子模块无需改 parent，外科手术式零侵入；IDEA 打开根即识别全部模块 |
| 包名 | **保留 `com.itheima`**（课程原包名） | 改名跨 tlias 几十文件，属投机重写，非必要 |
| JDK | JDK 21（`JAVA_HOME=D:/java/jre21.0.12` 跑 mvn） | 机器 PATH 上 `java`/`javac`=JDK 21，但 `mvn` 默认跑 JDK 8（`D:\java\jdk8`），会编不过 Java 11 target；与 javastudy 一致 |
| zip 处理 | 全部解压，剔除 `target/`、`*.iml`、编译产物，只留 `src`+`pom`+`resources` | 用户选「解压并清」；去除冗余、体积小、IDEA 干净导入 |
| 前端 day01-03 | 非 Maven，放 `frontend/` 目录，HTML/JS 原样 + `notes/` 拷 md | 用户选「连前端一起做」；day01 只有 PDF 讲义无 md |
| tlias | **只放 day15 多模块版一份**（=最终版，含 CRUD + 文件上传 + JWT + AOP） | 用户选「只放最终版一份」；day10/11/12/13 的 `tlias-web-management.zip` 不重复拷 |
| notes 对照 | 每个模块/目录下 `notes/` 拷对应 `讲义/*.md`（原样不改） | 与 javastudy 一致 |
| git | 初始化 + 按任务提交（同 javastudy 实际做法） | SDD 任务流天然按 task commit，便于回溯 |

### 1.1 环境注意

- PATH 上 `java`/`javac` = JDK 21（`21.0.12`）；Maven 3.9.16，但 Maven runtime = JDK 8（`D:\java\jdk8\jre`）。
- 命令行 `mvn` 编译必须 `JAVA_HOME=D:/java/jre21.0.12`；否则用 IDEA 内置编译（IDEA 用自配 Project SDK）。
- SpringBoot 2.7.4 子模块 pom 声明 `<java.version>11</java.version>`，JDK 21 编译 Java 11 target 无碍（向下兼容）。
- 依赖（SpringBoot/MyBatis/JWT 等）需联网从 Maven 中央仓库拉取。

## 2. 模块 / 目录结构

```
D:\javaproject\javaweb\
├── pom.xml                      # 聚合器 (packaging=pom, <modules> 列所有 Maven 子工程)
├── README.md
├── .gitignore
├── docs/                        # spec + plan (同 javastudy)
│
├── frontend/                    # 非 Maven (day01-03)
│   ├── day01-html-css/notes/    # 只有 PDF讲义/day01-HTML-CSS.pdf
│   ├── day02-js-vue/            # 23 个 *.html + notes/day02-JavaScript-Vue.md
│   └── day03-vue-element/       # ajax/*.html + element-ui/ + notes/day03_Vue_Element.md
│
├── day04-maven-springboot/      # 7 个 maven 小工程
│   ├── maven-project01~C/, http-server-demo/, springboot-web-quickstart/
│   └── notes/                    # Maven.md + SpringBootWeb入门.md
│
├── day05-req-resp/              # 4 迭代版本 (解压)
│   ├── 01-统一响应/ ~ 04-IOC详解/  # 各含 springboot-web-req-resp
│   └── notes/SpringBootWeb请求响应.md
│
├── day06-mysql/                 # 纯 SQL → notes/ 放 md + .sql 脚本 (非 Maven 模块)
├── day07-mysql/                 # 同上 (非 Maven 模块)
│
├── day08-mybatis/               # springboot-mybatis-quickstart 完整版 + notes/Mybatis入门.md
├── day09-mybatis/               # springboot-mybatis-crud + notes/Mybatis.md
│
├── tlias-final/                 # = day15/01.多模块开发 解压 (最终版 tlias)
│   ├── tlias-pojo/ tlias-utils/ tlias-web-management/  # 各自 pom, 嵌套 reactor
│   └── notes/                    # 案例-1/案例-2/登录认证/AOP 共 4 份 md 记演进
│
├── day13-aop/                   # springboot-aop-quickstart (独立 AOP 教学工程, 非 tlias)
├── day14-springboot-principle/ # 01-配置优先级 / 02-bean管理 / 03-自定义starter[2模块]
│   └── notes/SpringBoot原理篇.md
└── day15-maven-advanced/        # 02-继承与聚合 / 03-私服操作; (01 并入 tlias-final)
    └── notes/Maven高级.md
```

聚合器 `<modules>` 只列**有 pom 的 Maven 子工程**。前端目录、纯 SQL 的 day06/day07 作为普通目录（无 pom），不列入 `<modules>`，但仍在工程内便于对照。

### 2.1 聚合器 `<modules>` 清单（Maven 子工程）

按目录相对路径列出（实现时逐个核对 pom 存在性）：

- `day04-maven-springboot/maven-project01`
- `day04-maven-springboot/maven-project02`
- `day04-maven-springboot/maven-projectA`
- `day04-maven-springboot/maven-projectB`
- `day04-maven-springboot/maven-projectC`
- `day04-maven-springboot/http-server-demo`
- `day04-maven-springboot/springboot-web-quickstart`
- `day05-req-resp/01-统一响应/springboot-web-req-resp`
- `day05-req-resp/02-三层架构拆分/springboot-web-req-resp`
- `day05-req-resp/03-IOC入门/springboot-web-req-resp`
- `day05-req-resp/04-IOC详解/springboot-web-req-resp`
- `day08-mybatis/springboot-mybatis-quickstart`
- `day09-mybatis/springboot-mybatis-crud`
- `tlias-final/tlias-pojo`
- `tlias-final/tlias-utils`
- `tlias-final/tlias-web-management`
- `day13-aop/springboot-aop-quickstart`
- `day14-springboot-principle/01-配置优先级/springboot-web-config`
- `day14-springboot-principle/02-bean管理/springboot-web-config2`
- `day14-springboot-principle/03-自定义starter/aliyun-oss-spring-boot-autoconfigure`
- `day14-springboot-principle/03-自定义starter/aliyun-oss-spring-boot-starter`
- `day15-maven-advanced/02-继承与聚合` 下的子工程（实现时核对）
- `day15-maven-advanced/03-私服操作` 下的子工程（实现时核对）

> day05 的 4 个 zip 解压后顶层目录名都叫 `springboot-web-req-resp`，会落在各自带编号的父目录下避免冲突。day15 的 zip 内层目录可能含中文前缀（如 `01. 多模块开发/`），解压后按主题重命名为干净的模块目录。

## 3. 类命名 / 内容约定

- 不手写新 `.java`，不重命名课程类（保留 `com.itheima` 包、原类名如 `HelloController`、`DeptController`、`LogAspect` 等）。
- 唯一可能新增的文件：聚合器根 `pom.xml`、`README.md`、`.gitignore`、`notes/*.md`（拷贝）。
- 解压清理规则（剔除清单）：
  - `target/`（编译产物）
  - `*.iml`（IDEA 工程描述，导入时自动重建）
  - `.idea/`（如有）
  - `mvnw` / `mvnw.cmd` / `.mvn/`（保留与否均可，倾向剔除以保持干净；若子 pom 依赖 wrapper 则保留）

## 4. md 笔记对照拷贝映射

| 目标 notes/ | 源 md |
|---|---|
| frontend/day02-js-vue/notes/ | `day02-JavaScript-Vue/讲义/day02-JavaScript-Vue.md` + assets |
| frontend/day03-vue-element/notes/ | `day03-Vue-Element/讲义/day03_Vue_Element.md` + assets |
| day04-maven-springboot/notes/ | `day04.../讲义/01. Maven/Maven.md` + `02. SpringBootWeb入门/SpringBootWeb入门.md` |
| day05-req-resp/notes/ | `day05.../讲义/SpringBootWeb请求响应.md` + assets |
| day06-mysql/notes/ | `day06-MySQL/讲义/数据库-MySQL-01.md` + assets |
| day07-mysql/notes/ | `day07-MySQL/讲义/数据库-MySQL-02.md` + assets |
| day08-mybatis/notes/ | `day08.../讲义/02-Mybatis入门/Mybatis入门.md` + `01-MySQL/数据库-MySQL-03.md` |
| day09-mybatis/notes/ | `day09-Mybatis/讲义/Mybatis.md` + assets |
| tlias-final/notes/ | `day10.../讲义/SpringBootWeb案例-1.md` + `day11.../讲义/SpringBootWeb案例-2.md` + `day12.../讲义/SpringBootWeb登录认证.md` + `day13.../讲义/SpringBootWeb AOP.md` |
| day13-aop/notes/ | `day13.../讲义/SpringBootWeb AOP.md`（与 tlias 共用一份讲义；此处再拷一份方便对照） |
| day14-springboot-principle/notes/ | `day14.../讲义/SpringBoot原理篇.md` + assets |
| day15-maven-advanced/notes/ | `day15-maven高级/讲义/Maven高级.md` + assets |

day01 只有 `PDF讲义/day01-HTML-CSS.pdf`：拷到 `frontend/day01-html-css/notes/`。

> 讲义 md 引用 `assets/` 下的图片。拷 md 时**连同 `assets/` 一起拷**，保证 IDEA markdown 预览能显示图片。

## 5. 批次划分与成功标准

总目标（强成功标准）：**`JAVA_HOME=D:/java/jre21.0.12 mvn validate`（根聚合器）通过；每个含 Java 源码的 Maven 子模块 `mvn compile` 通过。**

分批，每批完成后验证：

| 批次 | 范围 | 验证 |
|---|---|---|
| 批 0 | 聚合器骨架：根 `pom.xml`（先空 `<modules>`）+ `.gitignore` + `README.md` + git init + docs/ | `mvn validate` 通过（空 modules） |
| 批 1 | day04 全部 7 个 maven 小工程解压清理 + notes | 加入 7 modules，`mvn validate` + 各 `mvn compile` |
| 批 2 | day05 4 版 + day08/day09 mybatis + notes | 加 modules，`mvn compile` |
| 批 3 | tlias-final（day15 多模块版）3 模块 + notes | reactor `mvn compile`（依赖 pojo/utils 自动排序） |
| 批 4 | day13-aop + day14（3 组）+ day15 剩余 + notes | `mvn compile` |
| 批 5 | 前端 day01-03 + 纯 SQL day06/day07 + notes | 浏览器可开 HTML；SQL 脚本就位 |

每批结束跑一次 `mvn validate`/`compile`（JAVA_HOME=JDK21）或 IDEA Rebuild，确认无编译错误再进下一批。

## 6. 范围与非目标

- **范围**：day01–15 全部课程工程解压整理成多模块聚合 + 前端 + 纯 SQL + notes 对照；tlias 只留 day15 多模块最终版。
- **非目标**：
  - 不改包名、不改业务代码（外科手术式）。
  - 不建库建表（tlias 等 MyBatis 工程编译不需 DB，但运行需建库；成功标准只保证编译通过）。
  - 不写额外单元测试（课程自带 test 类保留）。
  - 不为前端引入 Node/Vite 构建（原样 HTML，浏览器直接打开）。
  - 不重命名课程类、不重构。

## 7. 风险与诚实声明

- **tlias 运行需 MySQL**：`day09/代码/SQL脚本.txt`、`day10/资料` 含建表脚本。本工程只保证编译，不保证运行时 DB 就绪。
- **依赖需联网**：首次 `mvn compile` 会从中央仓库拉 SpringBoot/MyBatis/JJWT/dom4j 等依赖。离线会失败。
- **day15 zip 内层中文目录**：解压后需重命名为 Maven 合规 artifactId 风格的目录名（如 `多模块开发` → 已并入 tlias-final；`继承与聚合` → `inheritance-aggregation`），避免路径含中文影响部分工具。
- **SpringBoot 2.7.4 + JDK 21**：编译（source/target 11）兼容；但部分老旧依赖（如 jjwt 0.9.x）在 JDK 21 运行时可能有反射告警，不影响编译。
