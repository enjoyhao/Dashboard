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