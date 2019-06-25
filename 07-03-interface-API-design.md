# API - RESTful

【注】API前标有*号的预打算删除。

【注】返回类型说明中Res{}，省略字段为通用字段，只有Data会有所变化。

## A. 用户相关

此系统主要用来处理用户的相关信息。

### 1. 登陆服务（authentication）PORT=8001

**BASE_URL=http://localhost:8001/api/auth**

| 描述 | 方法 | API     | 参数 | 返回类型 |
| ---- | ---- | ------ | ---  | ----- |
| 登陆 | POST | http://localhost:8001/api/auth/user   | {code} | Res{,,{openid,token}} |
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
| *创建用户         | POST   | http://localhost:8003/api/users            |User|Res{,,[]User}|
| *删除用户         | DELETE | http://localhost:8003/api/users/{userId}   |nil|Res{,,nil}|

## B. 任务相关

此系统追踪任务的创建与发布，以及查询操作。（当查询所有任务时默认返回task简略信息，指明Id时返回详细信息）

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
| 创建任务                   | POST   | http://localhost:8005/api/tasks | 当前任务类型以问卷为主 | Wrapper{"questionnaire", Qtnr} | Res{,,Qtnr} |
| 修改任务                 | PUT   | http://localhost:8005/api/tasks/{taskId} | | Wrapper{"questionnaire", Qtnr} | Res{,,nil} |
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

| 描述             | 方法   | API                                                          | 参数 | 返回类型 |
| ---------------- | ------ | ------------------------------------------------------------ | ---------------- | ---------------- |
| 获取某个task的评论 | GET    | http://localhost:8006/api/tasks/{taskId}/comments   | nil | Res{,,[]Comment} |
| 发表评论         | POST   | http://localhost:8006/api/tasks/{taskId}/comments   | Comment | Res{,,Comment} |
| 更改某条评论     | PUT    | http://localhost:8006/api/tasks/{taskId}/comments/{cid} | Comment | Res{,,Comment} |
| 删除某条评论     | DELETE | http://localhost:8006/api/tasks/{taskId}/comments/{cid} | nil | Res{,,nil} |
| 点赞某条评论     | PUT    | http://localhost:8006/api/tasks/{taskId}/comments/{cid}/star | nil | Res{,,nil} |
| 取消点赞某条评论 | DELETE | http://localhost:8006/api/tasks/{taskId}/comments/{cid}/star | nil | Res{,,nil} |

**管理员API：**

| 描述         | 方法   | API                                                          | 参数 | 返回类型 |
| ------------ | ------ | ------------------------------------------------------------ | ------------ | ------------ |
| 查看某个task评论     | GET   | http://localhost:8006/api/tasks/{taskId}/comments       | nil | Res{,,[]Comment} |
| 删除某条评论 | DELETE | http://localhost:8006/api/tasks/{taskId}/comments/{cid} | nil | Res{,,nil} |

## C. 交易系统

此系统追踪发布者接受者双方任务的进行。

### 7. 用户交易（deal）PORT=8007

**查询用户相关交易：BASE_URL=http://localhost:8007/api/users/{userId}/deals**
**操作相关交易：BASE_URL=http://localhost:8007/api/deals**

**用户API：**

| 描述 | 方法 | API | 参数 | 返回类型 |
| ----- | ----- | ---- | ------ | ----- | ----- |
| 查看用户参与的所有交易 | GET | http://localhost:8007/api/deals | nil | Res{,,[]Deal} |
| 查看用户进行中的交易 | GET | http://localhost:8007/api/deals?state=underway | nil | Res{,,[]Deal} |
| 查看用户结束的交易| GET | http://localhost:8007/api/deals?state=closure | nil | Res{,,[]Deal} |
| 查看用户某笔交易 | GET | http://localhost:8007/api/deals/{dId} | nil | Res{,,Deal} |
| 删除结束的交易 | DELETE | http://localhost:8007/api/deals/{dId}| nil | nil |
| 创建交易（接受任务） | POST | http://localhost:8007/api/deals | Deal | Res{,,Deal} |

**管理员API：**

| 描述 | 方法 | API | 参数 | 返回类型 |
| ----- | ----- | ---- | ----- | ----- |
| 查看用户参与的所有交易 | GET | http://localhost:8007/api/users/{userId}/deals | nil | Res{,,[]Deal} |
| 查看用户进行中的交易 | GET | http://localhost:8007/api/users/{userId}/deals?state=underway | nil | Res{,,[]Deal} |
| 查看用户结束的交易| GET | http://localhost:8007/api/users/{userId}/deals?state=closure | nil | Res{,,[]Deal} |
| 查看所有交易 | GET | http://localhost:8007/api/deals | nil | Res{,,[]Deal} |
| 查看某笔交易 | GET | http://localhost:8007/api/deals/{dId} | nil | Res{,,[]Deal} |
| 查看所有尚未完成的交易 | GET | http://localhost:8007/api/deals?state=underway| nil | Res{,,[]Deal} |
| 查看所有结束的交易 | GET | http://localhost:8007/api/deals?state=closure | nil | Res{,,[]Deal} |

## D. 充值系统

此系统用于追踪用户充值操作以及查询余额信息等操作。

### 8. 充值信息（charge）PORT=8008

**查询用户充值信息：BASE_URL=http://localhost:8008/api/users/{userId}/charges**
**充值操作：BASE_URL=http://localhost:8008/api/charges**

**用户API：**

| 描述 | 方法 | API | 参数 | 返回类型 |
| ---- | ---- | ---- | ---- | ---- |
| 查看用户充值信息 | GET | http://localhost:8008/api/charges | nil | Res{,,[]Charge} |
| 账户充值 | POST | http://localhost:8008/api/charges | Charge | Res{,,Charge} |
| 查询某个充值记录 | GET | http://localhost:8008/api/charges/{cid} | nil | Res{,,Charge} |
| 删除充值信息 | DELETE | http://localhost:8008/api/charges/{cid} | nil | Res{,,nil} |

**管理员API：**

| 描述 | 方法 | API | 参数 | 返回类型 |
| ---- | ---- | ---- | ---- | ---- |
| 查看用户充值信息 | GET | http://localhost:8008/api/users/{userId}/charges | nil | Res{,,[]Charge} |
| 查询所有充值记录 | GET | http://localhost:8008/api/charges | nil | Res{,,[]Charge} |
| 查询某个充值记录 | GET | http://localhost:8008/api/charges/{cid} | nil | Res{,,Charge} |

## E. 商家任务审核系统

商家提交任务，经过系统审核方可放置到平台上。

**审核操作：BASE_URL=http://localhost:8009/api/reviews**

**商家API：**

| 描述                 | 方法 | API                                     | 参数   | 返回类型        |
| -------------------- | ---- | --------------------------------------- | ------ | --------------- |
| 提交审核             | POST | http://localhost:8009/api/reviews       | Review | Res{,,Review}   |
| 查看审核结果         | GET  | http://localhost:8009/api/reviews/{rid} | nil    | Res{,,Review}   |
| 查询当前商家所有审核 | GET  | http://localhost:8009/api/reviews       | nil    | Res{,,[]Review} |

**管理员API：**

| 描述         | 方法 | API                                     | 参数   | 返回类型        |
| ------------ | ---- | --------------------------------------- | ------ | --------------- |
| 修改审核状态 | PUT  | http://localhost:8009/api/reviews/{rid} | Review | Res{,,Review}   |
| 查询所有审核 | GET  | http://localhost:8009/api/reviews       | nil    | Res{,,[]Review} |


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
	TaskId        string          `json:"taskId" xorm:"taskId"`
	Query         []query         `json:"query" xorm:"query"`
	SingleChoice  []singleChoice  `json:"singleChoice" xorm:"singleChoice"`
	MutipleChoice []mutipleChoice `json:"mutipleChoice" xorm:"mutipleChoice"`
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

type mutipleChoice struct {
	Question string   `json:"question"`
	Choices  []string `json:"choices"`
	Answers  []string `json:"answers"`
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
	Id        string    `json:"id" xorm:"<-"`
	TaskId    string    `json:"taskId" xorm:"taskId"`
	Publisher string    `json:"publisher" xorm:"publisher"`
	Recipient string    `json:"recipient" xorm:"recipient"`
	Since     time.Time `json:"since" xorm:"since"`
	Until     time.Time `json:"until" xorm:"until"`
	Reward    float64   `json:"reward" xorm:"reward"`
	State     string    `json:"state" xorm:"state"`
}
```

**Comment属性表：**

```go
type Comment struct {
	Id         string    `json:"id" xorm:"<-"`
	TaskId     string    `json:"taskId" xorm:"taskId"`
	UserId     string    `json:"userId" xorm:"userId"`
	Timestamp  time.Time `json:"timestamp" xorm:"timestamp"`
	Content    string    `json:"content" xorm:"content"`
	Stars      int       `json:"stars" xorm:"stars"`
	Stargazers []string	 `json:"stargazers" xorm:"stargazers"`
}
```

**Charge属性表：**

```go
type Charge struct {
	Id 		  string 	`json:"id" xorm:"<-"`
	UserId    string    `json:"userId" xorm:"userId"`
	Amount 	  float64 	`json:"amount" xorm:"amount"`
	Timestamp time.Time `json:"timestamp" xorm:"timestamp"`
}
```

**Review属性表：**

```go
type Review struct {
	Id     string `json:"id" xorm:"<-"`
	TaskId string `json:"taskId" xorm:"taskId"`
	UserId string `json:"userId" xorm:"userId"`
	State  string `json:"state" xorm:"state"`
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