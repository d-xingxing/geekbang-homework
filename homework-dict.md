# 第一周作业

##### # **完成梯度压测案例：分析接口性能瓶颈**

* **要求 01**- 搭建好压测监控平台
* **要求 02**- 使用自己手里的项目的接口完成压测，分析出至少一个性能问题，并给出理由
* **要求 03**- 被测试的接口的性能指标 RT、TPS，系统资源（内存、CPU、磁盘 IO、网络 IO）使用
Grafana 观察取证
* **要求 04**- 按照 项目性能测试报告 格式提交作业
  * 参考资料中：项目性能测试报告.zip

---

# 第二周作业

##### 作业题：JVM 虚拟机论述题

作业一定要用自己的语言描述。复制粘贴可不是自己的！感觉自己这么说，能说明白了就达到标准
了！
题目 01- 请你用自己的语言向我介绍 Java 运行时数据区（内存区域）

- 堆、虚拟机栈、本地方法栈、方法区（永久代、元空间）、运行时常量池（字符串常量池）、直接内存
- 为什么堆内存要分年轻代和老年代？

题目 02- 描述一个 Java 对象的生命周期

- 解释一个对象的创建过程
- 解释一个对象的内存分配
- 解释一个对象的销毁过程
- 对象的 2 种访问方式是什么？
- 为什么需要内存担保？

题目 03- 垃圾收集算法有哪些？垃圾收集器有哪些？他们的特点是什么？

- ParNew 收集器
- ParallelScavenge 收集器
- ParallelOld 收集器
- CMS 收集器
- G1 收集器

---

# 第三周作业

## 作业题：JVM 虚拟机实践题

## 题目 - 请模拟服务高负载场景并观察 JVM 内部变化

主要关注：①Footprint 内存、②Throughput 吞吐量、③Latency 延迟

- Footprint：Eden、S0、S1，Old 内存区域占用及变化趋势
- Throughput：吞吐量
- Latency：GC 的 STW 引起的延迟

要求：

1. 要求：使用压力测试工具，给项目施加最大压力【低延时接口，高延时接口】
2. 要求：搭建 Grafana 的 JVM 监测的 Dashboard，采集各个指标信息
3. 要求：截图须附上的指标：RT、TPS、GC 的统计信息、堆内存统计信息、吞吐量
4. 要求：配置三种类型的 GC 组合：1. 吞吐量优先，2. 响应时间优先，3. 全功能垃圾收集器 G1
   **注**：监测工具可以自己选用，推荐使用 GCEasy、JDK 自带工具，Arthas，Grafana，Prometheus
   **JVM 配置参数的素材：**

```shell
# 吞吐量优先策略： 
JAVA_OPT="${JAVA_OPT} -Xms256m -Xmx256m -Xmn125m -XX:MetaspaceSize=128m - Xss512k" 
JAVA_OPT="${JAVA_OPT} -XX:+UseParallelGC -XX:+UseParallelOldGC "
JAVA_OPT="${JAVA_OPT} -XX:+PrintGCDetails -XX:+PrintGCTimeStamps - XX:+PrintGCDateStamps -XX:+PrintHeapAtGC -Xloggc:${BASE_DIR}/logs/gc-ps- po.log" 
 
# 响应时间优先策略 
JAVA_OPT="${JAVA_OPT} -Xms256m -Xmx256m -Xmn125m -XX:MetaspaceSize=128m - Xss512k" 
JAVA_OPT="${JAVA_OPT} -XX:+UseParNewGC -XX:+UseConcMarkSweepGC "
JAVA_OPT="${JAVA_OPT} -XX:+PrintGCDetails -XX:+PrintGCTimeStamps - XX:+PrintGCDateStamps -XX:+PrintHeapAtGC -Xloggc:${BASE_DIR}/logs/gc-parnew- cms.log" 
 
# 全功能垃圾收集器 
JAVA_OPT="${JAVA_OPT} -Xms256m -Xmx256m -XX:MetaspaceSize=128m -Xss512k"
JAVA_OPT="${JAVA_OPT} -XX:+UseG1GC -XX:MaxGCPauseMillis=100"
JAVA_OPT="${JAVA_OPT} -XX:+PrintGCDetails -XX:+PrintGCTimeStamps - XX:+PrintGCDateStamps -XX:+PrintHeapAtGC -Xloggc:${BASE_DIR}/logs/gc-g- one.log"
```

---

# 第四周作业

**题目 01- 请你说一说什么是线程和进程？**

- 区别
- 关系
- 线程的上下文切换是什么？
- 线程的并发与并行有啥区别？

**题目 02- 使用了多线程会带来什么问题呢？**

- 能不能详细说说线程安全问题？
- 原子性、有序性和可见性能不能深入的谈一下。

**题目 03- 什么是死锁？如何排查死锁?**
排查过程最好详细说明，最少说一种排查方案，越多越好。

---

# 第五周作业

## 作业题：网络编程实践题

## 题目 - 手写一个 Web 容器

**需求：工程师自定义一个 Web 容器提供给码农使用，码农只需要按照规定步骤，即可编写出自己的应用程序发布到 Web 容器**

### 01- 详细要求：

1. 要求：Web 容器支持发布静态资源到指定目录下
2. 要求：Web 容器支持动态资源 Servlet

### 02- 设计思路：

**Web 容器角色：**

- Web 容器开发者：工程师
- Web 容器使用者：码农
- 用户

**码农使用 Web 容器的步骤：**

- 码农编写应用程序：
    - 导入 Web 容器依赖坐标，并编写启动类
    - 将 index.html、js、image、css 等静态资源部署在 resources 目录下
    - 将自定义 Servlet 放置到指定包下：例如`com.hero.webapp`
- 码农发布自己的服务：
    - 码农将自己的接口 URL 按照固定规则发布：
        - 按照后缀，`.do`、`.action`、`无后缀`
    - 不管用何种规则：都将映射到自定义的 Servlet（类名映射，忽略大小写）举例

```
   http://localhost:8080/aaa/bbb/userservlet?name=xiong
```

- 用户在访问应用程序：
    - 按照 URL 地址访问服务
    - 如果没有指定的 Servlet，则访问默认的 Servlet
    - 如果访问的是静态资源，则读取文件输出给浏览器

**工程师实现 Web 容器思路：**

- 第一步：创建 Web 容器工程，导入依赖坐标
- 第二步：复制 HeroCat 中的简版 Servlet 规范：HeroRequest、HeroResponse、HeroServlet
- 第三步：实现 Servlet 规范
    - HttpCustomRequest
    - HttpCustomResponse
    - DefaultCustomServlet
- 第四步：编写 Web 容器核心代码：
    - CustomServer 基于 Netty 实现：Servlet 容器
    - CustomHandler 处理请求，映射到 Servlet 的容器的自定义 Servlet 中去
- 第五步：打包发布 Web 容器
- 第六步：测试容器

---

# 第六周作业

## 题目 01：

一条 SQL 语句在 MySQL 中是如何执行的？

## 题目 02

请解释一下你理解的事务是什么？
要点：

1. 事务四大特性 ACID
2. 事务隔离级别
3. 事务会产生的并发问题
4. 事务的安全性、性能与隔离级别的关系

---

# 第七周作业

题目 01- 完成 ReadView 案例，解释为什么 RR 和 RC 隔离级别下看到查询结果不一致
要求：

- 完成**案例 01- 读已提交 RC 隔离级别下的可见性分析**
- 完成**案例 02- 可重复读 RR 隔离级别下的可见性分析**
- 用通俗易懂的方式记录整个案例过程，可以画图与截图
- 做完案例给出结论，并对结论进行分析

回答范式：

1. 案例 01- 读已提交 RC 隔离级别下的可见性分析
    - 目标
    - 操作步骤
    - 实践过程
    - 结论
2. 案例 02- 可重复读 RR 隔离级别下的可见性分析
    - 目标
    - 操作步骤
    - 实践过程
    - 结论
3. 结论分析

## 题目 02- 什么是索引？

**要点：**

1. 优点是什么？
2. 缺点是什么？
3. 索引分类有哪些？特点是什么？
4. 索引创建的原则是什么？
5. 有哪些使用索引的注意事项？
6. 如何知道 SQL 是否用到了索引？
7. 请你解释一下索引的原理是什么？「重点」
    - 说清楚为什么要用 B+Tree

## 题目 03- 什么是 MVCC？

**要点：**

1. Redo 日志
2. ReadView
3. 如何判断可见性

---

# 第八周作业

## 题目 01- 请你说一说 MySQL 的锁机制

要求：

- 按照锁的粒度，锁的功能来分析
- 什么是死锁，为什么会发生，如何排查？
- 行锁是通过加在什么上完成的锁定？
- 详细说说这条 SQL 的锁定情况：`delete from tt where uid = 666`;

## 题目 02- 请你说一说 MySQL 的 SQL 优化



---

