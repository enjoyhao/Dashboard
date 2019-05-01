# API - RESTful

【注】API前标有*号的预打算删除。
【注】返回类型说明中Res{}，省略字段为通用字段，只有Data会有所变化。

## A. 用户相关

此系统主要用来处理用户的相关信息。

### 1. 登陆服务（authentication）PORT=8001

**BASE_URL=http://localhost:8001/api/auth**

| 描述 | 方法 | API     | 参数 | 返回类型 |
| ---- | ---- | ------ | ---  | ----- |
| 登陆 | POST | http://localhost:8001/api/auth/user   | | |
|      | POST | http://localhost:8001/api/auth/admin  | Admin | Res{,,nil} |
|      | POST | http://localhost:8001/api/auth/firm   | | |
| 退出 | GET  | http://localhost:8001/api/auth/logout | nil | Res{,,nil} |

### 2. 实名认证（certify）PORT=8002

**用户API：**
**BASE_URL=http://localhost:8002/api/users/{userId}/certs**

| 描述 | 方法 | API                                   |参数 | 返回类型 |
| -------------------------- | ------ | ----------- | --- | --- | 
| 认证（成功之后不允许修改）   | POST   | http://localhost:8002/api/users/{userId}/certs               | User | Res{,,User} |
|                            | GET    | http://locahost:8002/api/users/{userId}/certs                | nil | Res{,,User} |

**管理员API：**
**BASE_URL=http://localhost:8002/api/certs**
| 描述 | 方法 | API   |参数 | 返回类型 |
| ------------ | ------ | ----------------- |---  | ----- |
| 查看所有认证信息 | GET    | http://localhost:8002/api/certs | nil | Res{,,[]User} |
| 获取某个用户的认证信息 | GET    | http://localhost:8002/api/certs/{userId} | nil | Res{,,User} |
| 通过或者拒绝某个用户的认证信息 | POST   | http://localhost:8002/api/certs/{userId} | User | Res{,,User} |

### 3. 个人信息（user）PORT=8003

**BASE_URL=http://localhost:8003/api/users**

**用户API：**

| 描述                       | 方法   | API      |参数 | 返回类型|
| -------------------------- | ------ | ------------ | --- | --- |
| 个人信息                   | GET    | http://localhost:8003/api/users/{userId}  | nil | Res{,,User} |
|                            | PUT    | http://localhost:8003/api/users/{userId}  | User | Res{,,nil} |
| 注销                       | DELETE | http://localhost:8003/api/users/{userId}   |nil | Res{,,nil} |

**管理员API：**

| 描述             | 方法   | API                                        |参数 | 返回类型|
| ---------------- | ------ | ---- | --- | ---- |
| 查询所有用户     | GET    | http://localhost:8003/api/users            |nil|Res{,,[]User}|
| 查询某个用户     | GET    | http://localhost:8003/api/users/{userId}   |nil|Res{,,User}|
| 根据名称查询用户 | GET    | http://localhost:8003/api/users/{username} |nil|Res{,,[]User}|
| 创建用户         | POST   | http://localhost:8003/api/users            |User|Res{,,[]User}|
| 删除用户         | DELETE | http://localhost:8003/api/users/{userId}   |nil|Res{,,nil}|

## B. 任务相关

此系统追踪任务的创建与发布，以及查询操作。

### 4. 用户任务信息（task）PORT=8004

**BASE_URL=http://localhost:8004/api/users/{userId}/tasks**

**用户API：**

| 描述     | 方法   | API                                      |备注|参数|返回类型|
| ---------------- | ------ | ------------------- | ------- | --- | --- |
| 查询和自己创建的任务 | GET | http://localhost:8004/api/users/{userId}/tasks | userId为token中的userId | nil | Res{,,[]Task} |
| 查询某个用户发布的任务| GET | http://localhost:8004/api/users/{userId}/tasks?state=released | 不需要认证 | nil | Res{,,[]Task} |
| 查询自己未发布的任务|GET|http://localhost:8004/api/users/{userId}/tasks?state=non-released| userId为token中的userId |nil|Res{,,[]Task} |
| 查询自己关闭的任务|GET|http://localhost:8004/api/users/{userId}/tasks?state=closed| userId为token中的userId |nil|Res{,,[]Task} |

**管理员API：**

|描述|方法|API|参数|返回类型|
| ---------------- | ------ | -------- | --- | --- |
| 查询用户发起的所有任务 | GET  | http://localhost:8004/api/users/{userId}/tasks          |nil | Res{,,[]Task} |
| 查询用户关闭的任务| GET  | http://localhost:8004/api/users/{userId}/tasks?state=closed          |nil | Res{,,[]Task} |
| 查询用户发布的任务|GET|http://localhost:8004/api/users/{userId}/tasks?state=released|nil | Res{,,[]Task} |
| 查询用户未发布的任务|GET|http://localhost:8004/api/users/{userId}/tasks?state=non-released|nil | Res{,,[]Task} |

### 5. 任务交互（cpt）PORT=8005

**BASE_URL=http://localhost:8005/api/tasks**

**用户API：**

| 描述 | 方法 | API | 备注 |参数|返回类型|
| ---------------- | ------ | ----------------------------- | --------- | --- | --- |
| 查询所有任务 | GET | http://localhost:8005/api/tasks | 不需要认证 | nil | Res{,,[]Task} |
| 查询某个任务 | GET | http://localhost:8005/api/tasks/{taskId} | 不需要认证，指明任务Id时返回具体任务详情 | nil | Res{,,Qtnr} |
| 创建任务                   | POST   | http://localhost:8005/api/tasks | 当前任务类型以问卷为主 | nil | Res{,,Qtnr} |
| 修改任务                 | PUT   | http://localhost:8005/api/tasks/{taskId} | | Qtnr | Res{,,nil} |
| 删除关闭的任务           | DELETE | http://localhost:8005/api/tasks/{taskId}?state=closed | | nil | Res{,,nil} |
| 取消发布任务               | DELETE | http://localhost:8005/api/tasks/{taskId}?state=released | | nil | Res{,,nil} |
| 删除未发布的任务 | DELETE | http://localhost:8005/api/tasks/{taskId}?state=non-released | | nil | Res{,,nil} |

**管理员API：**

| 描述|方法|API|参数|返回类型|
| ---------------- | ------ | ----------------------------- | --- | --- |
| 查询所有任务 | GET | http://localhost:8005/api/tasks | nil | Res{,,[]Task} |
| 查询某个任务 | GET | http://localhost:8005/api/tasks/{taskId} | nil | Res{,,Qtnr} |
| 查询未发布的任务 | GET | http://localhost:8005/api/tasks?state=non-released| nil | Res{,,[]Task} |
| 查询发布的任务 | GET | http://localhost:8005/api/tasks?state=released| nil | Res{,,[]Task} |
| 查询关闭的任务| GET  | http://localhost:8005/api/tasks?state=closed  | nil | Res{,,[]Task} |

### 6. 任务评论（comment）PORT=8006

**BASE_URL=http://localhost:8006/api/tasks/{taskId}/comments**

**用户API：**

| 描述             | 方法   | API                                                          |
| ---------------- | ------ | ------------------------------------------------------------ |
| 获取某个task的评论 | GET    | http://localhost:8006/api/tasks/{taskId}/comments   |
| 发表评论         | POST   | http://localhost:8006/api/tasks/{taskId}/comments   |
| 更改某条评论     | PUT    | http://localhost:8006/api/tasks/{taskId}/comments/{cid} |
| 删除某条评论     | DELETE | http://localhost:8006/api/tasks/{taskId}/comments/{cid} |
| 点赞某条评论     | PUT    | http://localhost:8006/api/tasks/{taskId}/comments/{cid}/star |
| 取消点赞某条评论 | DELETE | http://localhost:8006/api/tasks/{taskId}/comments/{cid}/star |

**管理员API：**

| 描述         | 方法   | API                                                          |
| ------------ | ------ | ------------------------------------------------------------ |
| 查看某个task评论     | POST   | http://localhost:8006/api/tasks/{taskId}/comments       |
| 删除某条评论 | DELETE | http://localhost:8006/api/tasks/{taskId}/comments/{cid} |

## C. 交易系统

此系统追踪发布者接受者双方任务的进行。

### 7. 用户交易信息（deal）PORT=8007

**BASE_URL=http://localhost:8007/api/users/{userId}/deals**

**用户API：**

| 描述 | 方法 | API | 备注 |
| ----- | ----- | ---- | ------ |
| 查看用户相关所有交易 | GET | http://localhost:8007/api/users/{userId}/deals | userId为token中的userId |
| 查看用户某笔交易 | GET | http://localhost:8007/api/users/{userId}/deals/{dId} | userId为token中的userId |
| 查看正在进行中的交易 | GET | http://localhost:8007/api/users/{userId}/deals?state=underway | userId为token中的userId |
| 查看所有结束的交易| GET | http://localhost:8007/api/users/{userId}/deals?state=closure | userId为token中的userId |
| 删除结束的交易 | DELETE | http://localhost:8007/api/users/{userId}/deals/{dId}?state=closure | userId为token中的userId |

**管理员API：**

| 描述 | 方法 | API |
| ----- | ----- | ---- |
| 查看用户相关所有交易 | GET | http://localhost:8007/api/users/{userId}/deals |
| 查看正在进行中的交易 | GET | http://localhost:8007/api/users/{userId}/deals?state=underway |
| 查看所有结束的交易| GET | http://localhost:8007/api/users/{userId}/deals?state=closure |

### 8. 交易操作（txn）PORT=8008

**BASE_URL=http://localhost:8008/api/deals**

**用户API：**

| 描述 | 方法 | API |
| ----- | ------ | ------ |
| 发起交易 | POST | http://localhost:8008/api/deals |

**管理员API：**

| 描述 | 方法 | API |
| ---- | ---- | ---- |
| 查看所有交易 | GET | http://localhost:8008/api/deals |
| 查看某个交易 | GET | http://localhost:8008/api/deals/{dId} |
| 查看所有尚未完成的交易 | GET | http://localhost:8008/api/deals?state=underway|
| 查看所有结束的交易 | GET | http://localhost:8008/api/deals?state=closure |

## D. 充值系统

此系统用于追踪用户充值操作以及查询余额信息等操作。

### 9. 充值信息（balance）PORT=8009

**BASE_URL=http://localhost:8009/api/users/{userId}/balances**

**用户APi：**

| 描述 | 方法 | API |
| ---- | ---- | ---- |
| 查看用户充值信息 | GET | http://localhost:8009/api/users/{userId}/balances |
| 查看用户某条充值信息 | GET | http://localhost:8009/api/users/{userId}/balances/{bId} |
| 删除用户充值信息 | DELETE | http://localhost:8009/api/users/{userId}/balances/{bId} |

**管理员API：**

| 描述 | 方法 | API |
| ---- | ---- | ---- |
| 查看用户充值信息 | GET | http://localhost:8009/api/users/{userId}/balances |
| 查看用户某条充值信息 | GET | http://localhost:8009/api/users/{userId}/balances/{bId} |

### 10. 充值（recharge）PORT=8010

**BASE_URL=http://localhost:8010/api/balances**

**用户API：**

| 描述 | 方法 | API |
| ---- | --- | ---- |
| 账户充值 | POST | http://localhost:8010/api/balances |

**管理员API：**

| 描述 | 方法 | API |
| --- | --- | --- |
| 查询充值记录 | GET | http://localhost:8010/api/balances |
| 查询某个充值记录 | GET | http://localhost:8010/api/balances/{bId} |


## E. 商家任务审核系统

商家提交任务，经过系统审核方可放置到平台上。

## F. 工单系统（待定）


## Z. Model 数据结构说明

**Admin属性表：**
```go
type Admin struct {
	Name     string `json:"name"`
	Password string `json:"password"`
}
```

**User属性表：**
```go
type User struct {
	Id                  string  `json:"id"`
	SId                 string  `json:"sId" xorm:"sId"`
	Name                string  `json:"name"`
	Introduction        string  `json:"introduction"`
	Balance             float64 `json:"balance"`
	Icon                string  `json:"icon"`
	Phone               string  `json:"phone"`
	CreditScore         int     `json:"creditScore" xorm:"creditScore"`
	Email               string  `json:"email"`
	CertifiedPic        string  `json:"certifiedPic" xorm:"certifiedPic"`
	CertificationStatus int     `json:"certificationStatus" xorm:"certificationStatus"`
}
```

**Task元数据属性表：**
```go
// task状态、类型约定属性
const (
	TaskStateNonReleased  = "non-released"
	TaskStateReleased     = "released"
	TaskStateClosed       = "closed"
	TaskKindQuestionnaire = "questionnaire"
)

type Task struct {
	Id        string    `json:"id" xorm:"<-"`
	Kind      string    `json:"kind"`
	Publisher string    `json:"publisher"`
	Restrain  string    `json:"restrain"`
	Pubdate   time.Time `json:"pubdate"`
	Cutoff    time.Time `json:"cutoff"`
	Reward    float64   `json:"reward"`
	State     string    `json:"state"`
}
```

**Questionnaire属性表：**
```go
type Qtnr struct {
	Task
	Qtnr *Questionnaire `json:"qtnr"`
}

type Questionnaire struct {
	TaskId       string         `json:"taskId" xorm:"taskId"`
	Query        []query        `json:"query" xorm:"query"`
	SingleChoice []singleChoice `json:"singleChoice" xorm:"singleChoice"`
}

type query struct {
	Question string `json:"question"`
	Answer   string `json:"answer"`
}

type singleChoice struct {
	Question string   `json:"question"`
	Choices  []string `json:"choices"`
	Answer   string   `json:"answer"`
}
```

**Deal属性表：**
```go
// deal状态约定属性
const (
	DealStateUnderway = "underway"
	DealStateClosure  = "closure"
)

type Deal struct {
	TaskId    string    `json:"taskId" xorm:"taskId"`
	Publisher string    `json:"publisher" xorm:"publisher"`
	Recipient string    `json:"recipient" xorm:"recipient"`
	Since     time.Time `json:"since" xorm:"since"`
	Until     time.Time `json:"until" xorm:"until"`
	Reward    float64   `json:"reward" xorm:"reward"`
	State     string    `json:"state" xorm:"state"`
}
```

**Wrapper类型说明：**
wrapper目前主要是用于前端向后端发送任务数据，其中Kind为任务类型字段，raw为具体的任务字段（可以为Qtnr等等）。
```go
type Wrapper struct {
	Kind string          `json:"kind"`
	Raw  json.RawMessage `json:"raw"`
}
```

**Res返回类型说明：**
```go
type Resposne struct {
    Status  bool        `json:"status"`
    Errinfo string      `json:"errinfo"`
    Data    interface{} `json:"data"`
}
```