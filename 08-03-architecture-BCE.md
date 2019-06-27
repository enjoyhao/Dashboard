## 逻辑架构

逻辑架构由四层模型（表示层、业务层、服务层、持久化层）构成

### 表示层

- 学生端和机构端使用微信小程序作为表示层，提供任务查询子系统，发布任务子系统，参与任务子系统，充值提现子系统，个人信息管理子系统。
- 管理员端使用微信小程序作为表示层，提供

### 业务层

后端微服务充当业务层的角色，为表示层的各个子系统提供相应的服务模块，翻译用户的输入并依照用户的输入调用服务。

### 服务层

从各种数据访问对象检索和创建模型，更新各个存储库的值，执行特定于应用程序的逻辑和操作等。

### 持久化层

MySQL 提供了数据的持久化服务

## 框架目录设计

### 小程序

```
│  app.js
│  app.json
│  app.wxss
│  project.config.json
│  sitemap.json
│  weui.wxss
│
├─api
│      certify.js
│      charge.js
│      config.js
│      cpt.js
│      data.js
│      examples.js
│      index.js
│      task.js
│      test.js
│      user.js
│
├─components
│  ├─card
│  │      card.js
│  │      card.json
│  │      card.wxml
│  │      card.wxss
│  │
│  └─mini-tab
│         mini-tab.js
│         mini-tab.json
│         mini-tab.wxml
│         mini-tab.wxss
│
├─libs
│      runtime.js
│
├─mixins
│      link.js
│      question.js
│
├─pages
│  ├─design-questionnaire
│  │      design-questionnaire.js
│  │      design-questionnaire.json
│  │      design-questionnaire.wxml
│  │      design-questionnaire.wxss
│  │
│  ├─design-questionnaire-tmp
│  │      design-questionnaire.js
│  │      design-questionnaire.json
│  │      design-questionnaire.wxml
│  │      design-questionnaire.wxss
│  │
│  ├─designQuestionaire
│  │      designQuestionaire.js
│  │      designQuestionaire.json
│  │      designQuestionaire.wxml
│  │      designQuestionaire.wxss
│  │
│  ├─fillInQuestionaire
│  │      fillInQuestionaire.js
│  │      fillInQuestionaire.json
│  │      fillInQuestionaire.wxml
│  │      fillInQuestionaire.wxss
│  │
│  ├─index
│  │      index.js
│  │      index.json
│  │      index.wxml
│  │      index.wxss
│  │
│  ├─login
│  │      login.js
│  │      login.json
│  │      login.wxml
│  │      login.wxss
│  │
│  ├─logs
│  │      logs.js
│  │      logs.json
│  │      logs.wxml
│  │      logs.wxss
│  │
│  ├─mine
│  │      mine.js
│  │      mine.json
│  │      mine.wxml
│  │      mine.wxss
│  │
│  ├─newtask
│  │      newtask.js
│  │      newtask.json
│  │      newtask.wxml
│  │      newtask.wxss
│  │
│  ├─profile
│  │      profile.js
│  │      profile.json
│  │      profile.wxml
│  │      profile.wxss
│  │
│  ├─submitSuccess
│  │      submitSuccess.js
│  │      submitSuccess.json
│  │      submitSuccess.wxml
│  │      submitSuccess.wxss
│  │
│  └─taskDetail
│         taskDetail.js
│         taskDetail.json
│         taskDetail.wxml
│         taskDetail.wxss
│   
├─static
│  ├─icons
│  │      add.png
│  │      cancel.png
│  │      edit.png
│  │      publisher.png
│  │      reward.png
│  │
│  └─imgs
│          ad1.png
│          ad2.png
│          ad3.png
│          arrow_down.png
│          default-avatar.jpg
│
├─store
│      store.js
│
└─utils
        create.js
        diff.js
        promiseCalls.js
        util.js

```

## ECB
1. Boundary 对象：表示参与者与系统之间进行的交互以及信息交流
2. Controller 对象：一个用例具有的事件流的控制行为
3. Entity 对象：表示数据库中存储的信息及相关行为

### ECB中：

* Entity：代表系统数据，如：客户、订单、任务等
* Boundary：与用户的接口，如：界面、网关、代理等
* Controller：连接 Boundary 和 Entity 的媒介，编排来自 Boundary 的命令的执行

1. Entity 包含：
服务器框架目录设计中，model 目录下定义了所有服务器与 MySQL 相关的实体，承载系统数据
2. Controller 包含：
服务器框架目录设计中，controller 目录下定义了所有的相关内容，它们接收来自上述 Boundary 的命令，并调用 service 的对象来获取或更新模型或其他请求。
服务器框架目录设计中，service 目录下定义了部分相关内容，封装可能在控制器中的所有业务逻辑。这样，控制器就在那里转发和控制执行。
3. Boundary有三个部分：
管理员端 Web 程序用户界面
客户端小程序用户界面
服务端 Nginx 反向代理服务器

ECB是框架目录设计与逻辑架构的一种具体方法。