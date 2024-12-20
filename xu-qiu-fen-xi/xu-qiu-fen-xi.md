# 需求分析

## **1. 引言**

### **1.1 参考文献**

1. 李凯, 王涛. 《计算机程序设计与编程实践》. 高等教育出版社, 2020.
2. 张杰, 陈磊. 《软件工程：快速原型开发方法》. 清华大学出版社, 2018.
3. 顾维林. 《微服务架构：设计与实践》. 电子工业出版社, 2019.
4. Go-Judge: https://github.com/criyle/go-judge

### **1.2 软件项目描述**

本项目旨在开发一款面向编程初学者的在线评测系统，通过提供编程题目并使用自动化评测系统帮助学生提升编程能力。系统支持学生在线选择题目、提交代码，并实时获取评测反馈。教师能够布置题目并查看学生提交情况，且能够根据学生的表现给予个性化评价。本系统采用 Vue 3 和 Spring Boot 构建前后端，使用微服务架构和 Docker 部署，代码评测通过集成 go-judge 沙箱进行。

### **1.3 整体描述**

该项目目标是为编程初学者提供一个有效的编程学习平台，帮助学生通过实际编程练习增强实践能力，并通过即时反馈优化学习过程。项目主要模块包括：

* 用户认证与管理：支持学生和教师的注册、登录和权限管理。
* 题目管理：学生可以浏览和选择编程题目，教师可以布置题目。
* 代码评测：学生提交代码后，系统通过 go-judge 沙箱对代码进行自动评测，给出结果反馈。
* 前端界面：基于 Vue 3 实现响应式前端，提供用户友好的交互体验。
* 后端服务：基于 Spring Boot + Spring Cloud 架构，支持系统的高并发与可扩展性。

***

## **2. 信息描述**

### **2.1 信息内容**

**数据字典**

| 数据项              | 描述      | 数据类型     | 备注     |
| ---------------- | ------- | -------- | ------ |
| user\_id         | 用户唯一标识符 | Integer  | 主键     |
| username         | 用户名     | String   | 唯一     |
| password         | 用户密码    | String   | 密文存储   |
| role             | 用户角色    | String   | 学生/教师  |
| problem\_id      | 题目唯一标识符 | Integer  | 主键     |
| problem\_name    | 题目名称    | String   | 题目标题   |
| code\_submission | 提交的代码   | String   | 程序代码   |
| result           | 评测结果    | String   | 如 "通过" |
| submission\_time | 提交时间    | DateTime | 提交时间   |

**数据采集**

* 学生提交的代码通过 API 接口采集。
* 系统通过日志记录每次代码评测的结果和反馈。

**数据库描述**

* **用户表**：存储用户的基本信息，分为学生与教师，记录每个用户的登录信息和角色。
* **题目表**：包含所有可用的编程题目，每道题目包括题目标题、难度、描述等。
* **提交记录表**：记录每次代码提交的信息，包括提交的代码、评测结果、评测时间等。
* **评测结果表**：存储每次评测的结果，例如通过、失败、运行时错误等。

### **2.2 信息流**

**数据流**

* 用户注册时，系统向数据库中写入用户信息。
* 用户提交代码时，系统通过 API 接口将代码传递给评测模块，评测结果反馈至用户界面。
* 教师布置题目时，题目信息通过后端传输至数据库，并提供给学生选择。

**控制流**

* 用户注册后，系统控制流程为用户登录、选择题目、提交代码、评测并查看反馈。
* 教师的控制流为布置题目、查看学生提交情况、进行个性化评估。

***

## **3. 功能描述**

### **3.1 功能分解**

1. **用户管理模块**：
   * 用户注册与登录。
   * 权限控制：区分学生与教师角色。
2. **题目管理模块**：
   * 提供题目浏览与选择功能。
   * 教师能够发布新题目，更新题目状态。
3. **评测模块**：
   * 学生提交代码后，系统通过 go-judge 沙箱进行评测。
   * 提供评测结果反馈给学生，包括通过、失败及错误信息。
4. **前端界面**：
   * 提供响应式用户界面，支持用户交互和题目选择。
   * 提供代码编辑器与提交按钮，显示评测结果。

### **3.2 功能具体描述**

1. **用户管理**：
   * 学生通过注册功能输入个人信息，登录后可以查看题目并提交代码。
   * 教师通过登录后台查看学生提交情况并布置题目。
2. **题目管理**：
   * 学生可以浏览题库，选择感兴趣的题目进行解答。
   * 教师可以管理题目，包括题目的发布、修改和删除。
3. **代码评测**：
   * 学生提交代码后，后端系统将代码传输到 go-judge 沙箱进行评测，评测结果包括代码是否正确、执行时间等。
   * 系统根据评测结果反馈给学生，通过提示信息帮助学生改进代码。

### **3.3 处理说明**

1. **代码评测**：
   * 系统对代码进行黑盒测试，通过输入与输出进行比对。
   * 限制学生提交的代码文件大小、执行时间等，防止资源浪费。
2. **条件限制**：
   * 每道题目的提交次数有限制，防止滥用系统。
   * 系统需要处理并发提交，保证稳定性。
3. **性能需求**：
   * 系统需要支持至少 1000 个学生同时提交代码并获得评测结果。
   * 响应时间应控制在 5 秒以内，评测反馈时间控制在 10 秒以内。
4. **设计约束**：
   * 前端采用 Vue 3 技术栈，后端使用 Spring Boot 和 Spring Cloud 微服务架构。
   * 代码评测系统依赖于 go-judge 沙箱进行安全隔离。

### **3.4 控制描述**

* 前端通过 API 向后端发送请求，后端调用 go-judge 沙箱进行评测，最终将结果反馈给前端展示给用户。

***

## **4. 行为描述**

### **4.1 系统状态**

* 初始状态：系统未启动时，用户无法注册或登录。
* 活跃状态：用户能够正常注册、登录，浏览题目并提交代码。
* 维护状态：系统进行升级或维护时，用户无法提交代码。

### **4.2 事件和动作**

* 用户事件：注册、登录、选择题目、提交代码。
* 系统动作：用户提交代码后，系统进行评测并返回结果；教师发布新题目时，系统更新题库。

***

## **5. 确认标准**

### **5.1 性能范围**

1. **响应时间**：
   * 用户提交代码后，系统应在 **5秒** 内响应，并给出评测结果（包括成功、失败或错误提示）。
   * 前端与后端的交互响应时间不应超过 **2秒**，即用户操作后的反馈时间要在2秒以内。
2. **数据传输时间**：
   * 用户与服务器之间的数据传输时间应该低于 **1秒**，确保用户体验的流畅性。
   * 特别是对于用户提交的代码，数据传输过程需要经过加密和压缩，保障数据的快速传输。
3. **运行时间**：
   * 每次代码的评测执行时间应控制在 **10秒** 内，特别是在代码复杂度较高时，通过优化沙箱环境的配置来降低评测延迟。
   * 评测系统必须能够处理至少 **100** 个并发用户的请求，并保证评测结果的正确性与及时性。

### **5.2 测试种类（测试用例）**

1. **功能测试**：
   * 测试用户注册、登录、题目选择和代码提交功能是否正常工作。
   * 检查学生提交代码后，评测模块能正确评估代码，并提供相应的反馈结果。
   * 测试教师是否能够正确发布题目，并查看学生提交的情况。
2. **性能测试**：
   * 测试在高并发情况下，系统是否能够处理至少1000名学生同时提交代码，并且评测过程不受影响。
   * 测试评测模块的性能，包括沙箱的代码运行时间，确保不超过10秒。
3. **兼容性测试**：
   * 测试系统在不同操作系统和浏览器上的兼容性，确保系统支持主流浏览器（如 Chrome, Firefox, Edge）。
4. **安全测试**：
   * 测试系统的登录安全性，防止恶意用户利用漏洞进入后台管理。
   * 测试沙箱隔离功能，确保学生代码的运行不会危害系统的安全，避免恶意代码注入。
   * 测试数据传输是否加密，确保用户的个人信息和代码不会被泄露。
5. **用户体验测试**：
   * 测试前端界面的友好性和交互性，确保用户操作流畅、界面清晰。
   * 收集初期用户的反馈，优化界面和交互方式。

### **5.3 预期的软件响应：更新处理和数据转换**

1. **更新处理**：
   * 系统应支持在线更新，特别是在版本迭代或功能改进时，能够无缝地将新的功能模块推送至用户端。
   * 用户提交的代码和题目数据会定期同步至数据库，并自动更新处理。系统应支持批量处理题库更新和用户信息更新。
2. **数据转换**：
   * 系统需要处理并转换不同格式的数据，尤其是题目内容（如 Markdown 格式）、用户提交的代码（源代码）和评测结果（JSON 格式）。
   * 数据转换的过程中，应确保格式的一致性和准确性，特别是在评测结果反馈给用户时，需转换为易于理解的文本或图形格式。

### **5.4 特殊考虑**

1. **安全保密性**：
   * 系统需采用 HTTPS 协议加密用户数据传输，确保用户个人信息和代码数据不被泄露。
   * 对用户密码和其他敏感信息采用加密存储，防止数据库泄露带来的安全风险。
   * 使用沙箱技术隔离代码运行，防止用户代码对主机系统造成安全威胁。
   * 在用户操作中，采用验证码、防暴力破解等机制提高安全性。
2. **可维护性**：
   * 系统的代码结构要简洁、模块化，采用微服务架构进行分层设计，便于后期维护和功能扩展。
   * 系统文档和代码注释要完整，确保开发人员能够快速理解和修改代码。
   * 系统应定期进行版本更新和优化，解决潜在的性能瓶颈和安全问题。
3. **可移植性**：
   * 系统采用 Docker 容器化部署，确保能够在不同操作系统和硬件环境下运行。
   * 系统后端服务应具备一定的可移植性，支持多种数据库和配置方式，以便适应不同的运行环境。

***

## **6. 运行需求**

### **6.1 用户界面**

* **前端技术要求**：系统的前端将基于 Vue 3 开发，采用响应式设计，适应不同屏幕尺寸，确保在桌面、平板和手机上均能流畅使用。
* **交互要求**：提供简洁直观的用户界面，确保用户能够快速注册、登录，浏览题目，并提交代码。
* **支持浏览器**：系统应兼容主流浏览器，如 Google Chrome、Firefox、Edge 和 Safari，且界面显示无差异。

### **6.2 硬件接口**

* **服务器要求**：系统需运行在 Linux 或 Windows 服务器上，推荐使用云服务器（如 AWS、阿里云等）进行部署。
* **硬件配置要求**：至少支持 8 GB 内存，4 核 CPU，500 GB 存储空间，保障系统的并发处理能力和数据存储需求。
* **沙箱环境**：go-judge 沙箱部署时需要具有足够的硬件资源来运行多个并发代码评测任务，确保系统能够承载高并发量的代码评测。

### **6.3 软件接口**

* **数据库接口**：系统与 MySQL 或 PostgreSQL 数据库进行交互，存储用户信息、题目数据和评测记录。
* **RESTful API**：前端与后端通过 RESTful API 进行交互，进行数据传输与操作。
* **go-judge API**：通过调用 go-judge 提供的 API 接口对用户提交的代码进行评测。

### **6.4 故障处理**

* **异常处理**：系统应能捕获并记录运行时的异常，防止系统崩溃或功能中断。
* **日志管理**：所有系统日志（包括错误日志、请求日志、操作日志等）都应存储到中央日志系统，便于排查问题。
* **回滚机制**：当系统出现严重故障时，应能够支持数据回滚和版本回退，确保系统恢复到稳定状态。

***

## **7. 附录**

* **附录 A：技术栈**
  * 前端：Vue 3, Vuex, Axios
  * 后端：Spring Boot, Spring Cloud
  * 数据库：MySQL/PostgreSQL
  * 容器化部署：Docker, Kubernetes
  * 沙箱技术：go-judge
  * 版本控制：Git, GitHub
* **附录 B：术语定义**
  * **OJ（Online Judge）**：一种自动评测编程题目的系统，通过提交代码后自动评测其正确性和效率。
  * **沙箱（Sandbox）**：一种用于隔离和限制程序运行的环境，防止程序对系统造成影响。
  * **API（Application Programming Interface）**：应用程序编程接口，用于不同软件之间的交互。
* **附录 C：数据库表结构**
  * 用户表、题目表、提交记录表、评测结果表的具体字段与数据类型。
