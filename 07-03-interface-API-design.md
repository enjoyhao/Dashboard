# API - RESTful

【注】API前标有*号的预打算删除。

## A. 用户相关

### 1. 登陆服务（authentication）PORT=8001

**BASE_URL=http://localhost:8080/api/auth**

| 描述 | 方法 | API                                   |
| ---- | ---- | ------------------------------------- |
| 登陆 | POST | http://localhost:8080/api/auth/user   |
|      | POST | http://localhost:8080/api/auth/admin  |
|      | POST | http://localhost:8080/api/auth/firm   |
| 退出 | GET  | http://localhost:8080/api/auth/logout |

### 2. 实名认证（certify）PORT=8002

**BASE_URL=http://localhost:8080/api/certs**
**BASE_URL2=http://localhost:8080/api/users/{userId}/certs**

**用户API：**

| 描述 | 方法 | API                                   |
| -------------------------- | ------ | ------------------------------------------------------------ |
| 认证（成功之后不允许修改）   | POST   | http://localhost:8080/api/users/{userId}/certs               |
|                            | GET    | http://locahost:8080/api/users/{userId}/certs                |

**管理员API：**

| 描述 | 方法 | API                                   |
| ------------ | ------ | ----------------- |
| 查看所有认证信息 | GET    | http://localhost:8080/api/certs |
| 获取某个用户的认证信息 | GET    | http://localhost:8080/api/certs/{userId} |
| 通过或者拒绝某个用户的认证信息 | POST   | http://localhost:8080/api/certs/{userId} |

### 3. 个人信息（user）PORT=8003

**BASE_URL=http://localhost:8080/api/users**

**用户API：**

| 描述                       | 方法   | API                                                          |
| -------------------------- | ------ | ------------------------------------------------------------ |
| 个人信息                   | GET    | http://localhost:8080/api/users/{userId}                     |
|                            | PUT    | http://localhost:8080/api/users/{userId}                     |
| 注销                       | DELETE | http://localhost:8080/api/users/{userId}                     |

**管理员API：**

| 描述             | 方法   | API                                        |
| ---------------- | ------ | ------------------------------------------ |
| 查询所有用户     | GET    | http://localhost:8080/api/users            |
| 查询某个用户     | GET    | http://localhost:8080/api/users/{userId}   |
| 根据名称查询用户 | GET    | http://localhost:8080/api/users/{username} |
| 创建用户         | POST   | http://localhost:8080/api/users            |
| 删除用户         | DELETE | http://localhost:8080/api/users/{userId}   |

## B. 任务相关

此系统追踪任务的创建与发布，以及查询操作。

### 4. 用户任务信息（task）PORT=8004

**BASE_URL=http://localhost:8080/api/users/{userId}/tasks**

**用户API：**

| 描述     | 方法   | API                                      |备注|
| ---------------- | ------ | -------------------------------- | ------- |
| 查询某个用户发布的任务| GET | http://localhost:8080/api/users/{userId}/tasks?state=released | 不需要认证 |
| 查询和自己相关的所有任务 | GET | http://localhost:8080/api/users/{userId}/tasks | userId为token中的userId |
| 查询自己未发布的任务|GET|http://localhost:8080/api/users/{userId}/tasks?state=non-released| userId为token中的userId |

**管理员API：**

|描述|方法|API|
| ---------------- | ------ | ----------------------------- |
| 查询用户发起的所有任务 | GET  | http://localhost:8080/api/users/{userId}/tasks          |
| 查询已经完成的任务| GET  | http://localhost:8080/api/users/{userId}/tasks?state=finished          |
| 查询已经发布但是未被领取的任务|GET|http://localhost:8080/api/users/{userId}/tasks?state=released|
| 查询未发的任务|GET|http://localhost:8080/api/users/{userId}/tasks?state=non-released|

### 5. 任务交互（cpt）PORT=8005

**BASE_URL=http://localhost:8080/api/tasks**

**用户API：**

| 描述 | 方法 | API | 备注 |
| ---------------- | ------ | ----------------------------- | --------- |
| 查询所有任务 | GET | http://localhost:8080/api/tasks | 不需要认证 |
| 查询某个任务 | GET | http://localhost:8080/api/tasks/{taskId} | 不需要认证 |
| 创建任务                   | POST   | http://localhost:8080/api/tasks | 当前任务类型以问卷为主 |
| 修改任务                 | PUT   | http://localhost:8080/api/tasks/{taskId} | |
| 删除已完成的任务           | DELETE | http://localhost:8080/api/tasks/{taskId}?state=finished | |
| 取消发布任务               | DELETE | http://localhost:8080/api/tasks/{taskId}?state=released | |
| 删除未发布的任务 | DELETE | http://localhost:8080/api/tasks/{taskId}?state=non-released | |

**管理员API：**

| 描述|方法|API|
| ---------------- | ------ | ----------------------------- |
| 查询所有任务 | GET | http://localhost:8080/api/tasks |
| 查询某个任务 | GET | http://localhost:8080/api/tasks/{taskId} |
| 查询未发布的任务 | GET | http://localhost:8080/api/tasks?state=non-released|
| 查询已经发布但是未被领取的任务 | GET | http://localhost:8080/api/tasks?state=released|
| 查询所有正在进行中的任务 | GET | http://localhost:8080/api/tasks?state=claimed|
| 查询已经完成的任务| GET  | http://localhost:8080/api/tasks?state=finished          |


### 6. 任务评论（comment）PORT=8006

**BASE_URL=http://localhost:8080/api/tasks/{taskId}/comments**

**用户API：**

| 描述             | 方法   | API                                                          |
| ---------------- | ------ | ------------------------------------------------------------ |
| 获取某个task的评论 | GET    | http://localhost:8080/api/tasks/{taskId}/comments   |
| 发表评论         | POST   | http://localhost:8080/api/tasks/{taskId}/comments   |
| 更改某条评论     | PUT    | http://localhost:8080/api/tasks/{taskId}/comments/{cid} |
| 删除某条评论     | DELETE | http://localhost:8080/api/tasks/{taskId}/comments/{cid} |
| 点赞某条评论     | PUT    | http://localhost:8080/api/tasks/{taskId}/comments/{cid}/star |
| 取消点赞某条评论 | DELETE | http://localhost:8080/api/tasks/{taskId}/comments/{cid}/star |

**管理员API：**

| 描述         | 方法   | API                                                          |
| ------------ | ------ | ------------------------------------------------------------ |
| 查看某个task评论     | POST   | http://localhost:8080/api/tasks/{taskId}/comments       |
| 删除某条评论 | DELETE | http://localhost:8080/api/tasks/{taskId}/comments/{cid} |

## C. 交易系统

此系统追踪发布者接受者双方任务的进行。

### 7. 用户交易信息（deal）PORT=8007

**BASE_URL=http://localhost:8080/api/users/{userId}/deals**

**用户API：**

| 描述 | 方法 | API | 备注 |
| ----- | ----- | ---- | ------ |
| 查看用户相关所有交易 | GET | http://localhost:8080/api/users/{userId}/deals | userId为token中的userId |
| 查看用户某笔交易 | GET | http://localhost:8080/api/users/{userId}/deals/{dId} | userId为token中的userId |
| 查看正在进行中的交易 | GET | http://localhost:8080/api/users/{userId}/deals?state=underway | userId为token中的userId |
| 查看所有结束的交易| GET | http://localhost:8080/api/users/{userId}/deals?state=closure | userId为token中的userId |
| 删除结束的交易 | DELETE | http://localhost:8080/api/users/{userId}/deals/{dId}?state=closure | userId为token中的userId |

**管理员API：**

| 描述 | 方法 | API |
| ----- | ----- | ---- |
| 查看用户相关所有交易 | GET | http://localhost:8080/api/users/{userId}/deals |
| 查看正在进行中的交易 | GET | http://localhost:8080/api/users/{userId}/deals?state=underway |
| 查看所有结束的交易| GET | http://localhost:8080/api/users/{userId}/deals?state=closure |

### 8. 交易操作（txn）PORT=8008

**BASE_URL=http://localhost:8080/api/deals**

**用户API：**

| 描述 | 方法 | API |
| ----- | ------ | ------ |
| 发起交易 | POST | http://localhost:8080/api/deals |

**管理员API：**

| 描述 | 方法 | API |
| ---- | ---- | ---- |
| 查看所有交易 | GET | http://localhost:8080/api/deals |
| 查看某个交易 | GET | http://localhost:8080/api/deals/{dId} |
| 查看所有尚未完成的交易 | GET | http://localhost:8080/api/deals?state=underway|
| 查看所有结束的交易 | GET | http://localhost:8080/api/deals?state=closure |

### 9. 充值信息（balance）PORT=8009

**BASE_URL=http://localhost:8080/api/users/{userId}/balances**

**用户APi：**

| 描述 | 方法 | API |
| ---- | ---- | ---- |
| 查看用户充值信息 | GET | http://localhost:8080/api/users/{userId}/balances |
| 查看用户某条充值信息 | GET | http://localhost:8080/api/users/{userId}/balances/{bId} |
| 删除用户充值信息 | DELETE | http://localhost:8080/api/users/{userId}/balances/{bId} |

**管理员API：**

| 描述 | 方法 | API |
| ---- | ---- | ---- |
| 查看用户充值信息 | GET | http://localhost:8080/api/users/{userId}/balances |
| 查看用户某条充值信息 | GET | http://localhost:8080/api/users/{userId}/balances/{bId} |

### 10. 充值（recharge）PORT=8010

**BASE_URL=http://localhost:8080/api/balances**

**用户API：**

| 描述 | 方法 | API |
| ---- | --- | ---- |
| 账户充值 | POST | http://localhost:8080/api/balances |

**管理员API：**

| 描述 | 方法 | API |
| --- | --- | --- |
| 查询充值记录 | GET | http://localhost:8080/api/balances |
| 查询某个充值记录 | GET | http://localhost:8080/api/balances/{bId} |


## D. 商家任务审核系统

商家提交任务，经过系统审核方可放置到平台上。

## E. 工单系统（待定）

