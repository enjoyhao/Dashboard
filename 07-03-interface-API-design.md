# API - RESTful

## 0. 认证服务

| 描述 | 方法 | API                                   |
| ---- | ---- | ------------------------------------- |
| 登陆 | POST | http://localhost:8080/api/auth/user   |
|      | POST | http://localhost:8080/api/auth/admin  |
|      | POST | http://localhost:8080/api/auth/firm   |
| 退出 | GET  | http://localhost:8080/api/auth/logout |

## A. 用户相关

**BASE_URL=http://localhost:8080/api/users**

**用户API：**

| 描述                       | 方法   | API                                                          |
| -------------------------- | ------ | ------------------------------------------------------------ |
| 认证（不允许修改、仅一次） | POST   | http://localhost:8080/api/users/{userid}/auth                |
|                            | GET    | http://locahost:8080/api/users/{userid}/auth                 |
| 个人信息                   | GET    | http://localhost:8080/api/users/{userid}                     |
|                            | POST   | http://localhost:8080/api/users/{userid}(可去掉)             |
|                            | PUT    | http://localhost:8080/api/users/{userid}                     |
| 注销                       | DELETE | http://localhost:8080/api/users/{userid}                     |
| 发布任务                   | POST   | http://localhost:8080/api/users/{userid}/tasks               |
| 领取任务                   | POST   | http://localhost:8080/api/users/{userid}/tasks/{taskid}      |
| 取消领取任务               | DELETE | http://localhost:8080/api/users/{userid}/tasks/{taskid}?status=claimed |
| 删除已完成的任务           | DELETE | http://localhost:8080/api/users/{userid}/tasks/{taskid}?status=finished |
| 取消发布任务               | DELETE | http://localhost:8080/api/users/{userid}/tasks/{taskid}?status=released |

**管理员API：**

| 描述     | 方法   | API                                      |
| -------- | ------ | ---------------------------------------- |
| 管理用户 | GET    | http://localhost:8080/api/users          |
|          | GET    | http://localhost:8080/api/users/{userid} |
|          | POST   | http://localhost:8080/api/users          |
|          | DELETE | http://localhost:8080/api/users/{userid} |
|          | GET    | http://localhost:8080/api/auths          |
|          | GET    | http://localhost:8080/api/auths/{userid} |

**公共API：**

| 描述                   | 方法 | API                                                     |
| ---------------------- | ---- | ------------------------------------------------------- |
| 修改任务               | PUT  | http://localhost:8080/api/users/{userid}/tasks/{taskid} |
| 查询用户发起的某个任务 | GET  | http://localhost:8080/api/users/{userid}/tasks/{taskid} |
| 查询用户发起的所有任务 | GET  | http://localhost:8080/api/users/{userid}/tasks          |

## B. 任务相关

**BASE_URL=http://localhost:8080/api/tasks**

**用户API：**

| 描述             | 方法   | API                                                          |
| ---------------- | ------ | ------------------------------------------------------------ |
| 发表评论         | POST   | http://localhost:8080/api/tasks/{userid}/{taskid}/comments   |
| 更改某条评论     | PUT    | http://localhost:8080/api/tasks/{userid}/{taskid}/comments/{cid} |
| 点赞某条评论     | PUT    | http://localhost:8080/api/tasks/{userid}/{taskid}/comments/{cid}/star |
| 取消点赞某条评论 | DELETE | http://localhost:8080/api/tasks/{userid}/{taskid}/comments/{cid}/star |
| 开设issue        | POST   | http://localhost:8080/api/tasks/{userid}/{taskid}/issues     |
| 更改某个issue    | PUT    | http://localhost:8080/api/tasks/{userid}/{taskid}/issues/{iid} |
| 关闭某个issue    | PUT    | http://localhost:8080/api/tasks/{userid}/{taskid}/issues/{iid}/lock |
| 打开某个issue    | DELETE | http://localhost:8080/api/tasks/{userid}/{taskid}/issues/{iid}/lock |
| 领取任务         | PUT    | http://localhost:8080/api/tasks/{userid}/{taskid}            |
| 完成任务         | PUT    | http://localhost:8080/api/tasks/{userid}/{taskid}            |

**公共API：**

| 描述                   | 方法   | API                                                          |
| ---------------------- | ------ | ------------------------------------------------------------ |
| 获取某个task的评论     | GET    | http://localhost:8080/api/tasks/{userid}/{taskid}/comments   |
| 获取某个task的某条评论 | GET    | http://localhost:8080/api/tasks/{userid}/{taskid}/comments/{cid} |
| 删除某条评论           | DELETE | http://localhost:8080/api/tasks/{userid}/{taskid}/comments/{cid} |
| 获取所有issue          | GET    | http://localhost:8080/api/tasks/{userid}/{taskid}/issues     |
| 获取某个issue          | GET    | http://localhost:8080/api/tasks/{userid}/{taskid}/issues/{iid} |
| 删除某个issue          | DELETE | http://localhost:8080/api/tasks/{userid}/{taskid}/issues/{iid} |
| 查询所有任务           | GET    | http://localhost:8080/api/tasks                              |
| 查询某个任务           | GET    | http://localhost:8080/api/tasks/{taskid}                     |

**商家API：**

| 描述         | 方法   | API                                                          |
| ------------ | ------ | ------------------------------------------------------------ |
| 发表评论     | POST   | http://localhost:8080/api/tasks/{userid}/{taskid}/comments       |
| 更改某条评论 | PATCH  | http://localhost:8080/api/tasks/{userid}/{taskid}/comments/{cid} |
| 删除某条评论 | DELETE | http://localhost:8080/api/tasks/{userid}/{taskid}/comments/{cid} |

## C. 交易系统

## D. 授权审核系统

商家提交任务，经过系统审核方可放置到平台上。

## E. 工单系统（待定）

