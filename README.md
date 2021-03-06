# 前端监控

## 前端监控类型
* 行为监控
* 异常监控 (包括错误监控)
* 性能监控

## 监控的步骤
1. 日志采集
2. 日志存储
3. 日志分析
4. 报警


- 采集阶段：收集异常日志，先在本地做一定的处理，采取一定的方案上报到服务器。

- 存储阶段：后端接收前端上报的异常日志，经过一定处理，按照一定的存储方案存储。

- 分析阶段：分为机器自动分析和人工分析。机器自动分析，通过预设的条件和算法，对存储的日志信息进行统计和筛选，发现问题，触发报警。人工分析，通过提供一个可视化的数据面板，让系统用户可以看到具体的日志数据，根据信息，发现异常问题根源。

- 报警阶段：分为告警和预警。告警按照一定的级别自动报警，通过设定的渠道，按照一定的触发规则进行。预警则在异常发生前，提前预判，给出警告。


## 常见的错误
1. 逻辑的错误
2. 数据类型错误
3. 语法错误
4. 网络错误
5. 系统错误

## 采集信息分类
1. 用户信息(用户权限,当前时刻,终端信息)
2. 行为信息(操作路径,操作的数据)
3. 异常信息(Dom节点.异常级别,异常的类型,代码的stack信息)
4. 环境信息(网络环境,设备型号标识码 操作系统版本,客户端版本,API接口版本)

## 信息字段
| 字段           | 类型               | 解释                                                                       |
| -------------- | ------------------ | -------------------------------------------------------------------------- |
| requestId      | String             | 一个界面产生一个requestId                                                  |
| traceId        | String             | 一个阶段产生一个traceId，用于追踪和一个异常相关的所有日志记录              |
| hash           | String             | 这条log的唯一标识码，相当于logId，但它是根据当前日志记录的具体内容而生成的 |
| time           | Number             | 当前日志产生的时间（保存时刻）                                             |
| userId         | String             |                                                                            |
| userStatus     | Number             | 当时，用户状态信息（是否可用/禁用）                                        |
| userRoles      | Array              | 当时，前用户的角色列表                                                     |
| userGroups     | Array              | 当时，用户当前所在组，组别权限可能影响结果                                 |
| userLicenses   | Array              | 当时，许可证，可能过期                                                     |
| path           | String             | 所在路径，URL                                                              |
| action         | String             | 进行了什么操作                                                             |
| referer        | String             | 上一个路径，来源URL                                                        |
| prevAction     | String             | 上一个操作                                                                 |
| data           | Object             | 当前界面的state、data                                                      |
| dataSources    | Array<Object>      | 上游api给了什么数据                                                        |
| dataSend       | Object             | 提交了什么数据                                                             |
| targetElement  | HTMLElement        | 用户操作的DOM元素                                                          |
| targetDOMPath  | Array<HTMLElement> | 该DOM元素的节点路径                                                        |
| targetCSS      | Object             | 该元素的自定义样式表                                                       |
| targetAttrs    | Object             | 该元素当前的属性及值                                                       |
| errorType      | String             | 错误类型                                                                   |
| errorLevel     | String             | 异常级别                                                                   |
| errorStack     | String             | 错误stack信息                                                              |
| errorFilename  | String             | 出错文件                                                                   |
| errorLineNo    | Number             | 出错行                                                                     |
| errorColNo     | Number             | 出错列位置                                                                 |
| errorMessage   | String             | 错误描述（开发者定义）                                                     |
| errorTimeStamp | Number             | 时间戳                                                                     |
| eventType      | String             | 事件类型                                                                   |
| pageX          | Number             | 事件x轴坐标                                                                |
| pageY          | Number             | 事件y轴坐标                                                                |
| screenX        | Number             | 事件x轴坐标                                                                |
| screenY        | Number             | 事件y轴坐标                                                                |
| pageW          | Number             | 页面宽度                                                                   |
| pageH          | Number             | 页面高度                                                                   |
| screenW        | Number             | 屏幕宽度                                                                   |
| screenH        | Number             | 屏幕高度                                                                   |
| eventKey       | String             | 触发事件的键                                                               |
| network        | String             | 网络环境描述                                                               |
| userAgent      | String             | 客户端描述                                                                 |
| device         | String             | 设备描述                                                                   |
| system         | String             | 操作系统描述                                                               |
| appVersion     | String             | 应用版本                                                                   |
| apiVersion     | String             | 接口版本                                                                   |

## 前端异常捕获
1. 全局捕获
* 全局的接口捕获代码集中管理 
 - window.addEventListener("error") 
 - window.addEventListener("unhandlerejection")
 - document.addEventListener('click')等
 - 框架级别的全局监听,如 axios 中使用 interceptor 拦截
 - 全局函数的封装包裹,实现调用该函数自动捕获异常
 - 对实例方法重写, 在原有功能基础上包裹一层,如 console.error console.warn 重写,保持原先使用方法不变的情况下,捕获异常

2. 单点捕获
   1. 在业务代码中对单个代码块进行包裹,逻辑流程中埋点,实现针对性的异常捕获
      1. try...catch
      2. 专门函数收集错误信息,异常发生时, 调用该函数
      3. 写一个函数包裹其他函数,得到一个新函数

## 异常录制
![avatar](https://cdc.tencent.com/wp-content/uploads/2018/09/Screen-Shot-2018-09-13-at-5.24.36-PM.png)


## 前端存储日志
1. 选用indexedDB 
   1. 容量大
   2. 异步调用,不对界面渲染产生阻塞

## 前端整理日志
1. 按照时序存放在归档区，并将新入库的日志加入索引
2. BatchIndexes : 批量上报索引(包含性能等其他日志), 可一次批量上传100条
3. MomentIndexes : 即时上报索引, 一次全部上报
4. FeedbackIndexes : 用户反馈索引,一次上报一条
5. BlockIndexes :  区块上报索引,按异常 /  错误(traceId,requestId) 分块,一次上报一块
6. 上报完成把已上报的删除
7. 3天以上放入回收区
8. 7天以上从回收区删除
* requestId：同时追踪前后端日志。由于后端也会记录自己的日志，因此，在前端请求api的时候，默认带上requestId，后端记录的日志就可以和前端日志对应起来。```

*  traceId  追踪一个异常发生前后的相关日志当应用启动时，创建一个traceId，直到一个异常发生时，刷新traceId。把一个traceId相关的requestId收集起来，把这些requestId相关的日志组合起来，就是最终这个异常相关的所有日志，用来对异常进行复盘

## 上报日志
* 上报日志可以在 Web Worker 中执行 不阻塞主进程,为了和**整理 和 区分**，可以分两个worker。上报的流程大致为：在每一个循环中，从索引区取出对应条数的索引，通过索引中的hash，到归档区取出完整的日志记录，再上传到服务器。
* 错误上报按紧急程度分类
  * 即时上报
  * 批量上报
  * 区块上报
  * 用户主动提交

## 压缩上报数据
1. 可以采用gzip
2. lz-string是一个非常优秀的字符串压缩类库，兼容性好，代码量少，压缩比高，压缩时间短，压缩率达到惊人的60%。


## 日志统计与分析
* 用户维度
  * 同一个用户的不同请求实际上会形成不同的story线，因此，针对用户的一系列操作设计唯一的request id是有必要的。同一个用户在不同终端进行操作时，也能进行区分。用户在进行某个操作时的状态、权限等信息，也需要在日志系统中予以反应。 
* 时间维度
  * 一个异常是怎么发生的，需要将异常操作的前后story线串联起来观察。它不单单涉及一个用户的一次操作，甚至不限于某一个页面，而是一连串事件的最终结果。
* 性能维度
  * 界面加载市场,API请求时长统计,单元计算耗时统计,用户呆滞时长
* 运行环境维度
  * 应用所在的网络环境，操作系统，设备硬件信息等，服务器cpu、内存状况，网络、宽带使用情况等。
* 代码追踪 
  * 异常代码的stack 信息,定位到发生异常的代码位置和异常堆栈
* 场景回溯
  * 

## 修复异常
1. sourceMap 
   1. 压缩代码js 部署在服务器上,sourcemap 文件上传在监控系统中,监控系统展示stack信息的时候,利用sourcemap 对stack 信息解码,得到源码中的具体信息
   2. sourcemap 必须和正式环境的版本对应,还必须与git 的commit节点对应,这样才能保证查异常的时候可以正确利用stack 信息,找出问题所在版本的代码,可以通过建立CI任务,在集成化部署中增加一个部署流程,实现这一环节

## 异常测试
1. 主动异常测试
   1. 每次发现一个异常就将它加入异常用例列表中
   2.  随机异常测试, 模拟真实环境，在模拟器中模拟真实用户的随机操作，利用自动化脚本产生随机操作动作代码，并执行。

## 性能监控
1. 运行时性能: 文件级别 ,模块级别 ,函数级别,算法级别
2. 网络请求速率
3. 系统性能

## API Monitor
1. 稳定性监控
2. 数据格式和类型
3. 报错监控
4. 数据准确性监控

## 数据脱敏
敏感数据不被日志系统采集

* 应用如果涉及敏感数据
  1. 独立部署
  2. 不采集具体数据,只采集用户操作数据,在重现时候,通过日志信息可以去除数据api 结果来展示
  3. 日志加密,做到软硬件层面的加密防护
  4. 可以mock数据
  5. 对敏感数据进行混淆
