# REST API设计规范

## 一、什么是REST？

### 1、资源Resources

**REST（Representational State Transfer表征状态转移）**描述的主体是“资源”（Resources），“资源”是网络上的一个实体，可以是一段文本，一张图片等等。我们通常使用URI（统一资源定位符）指向它，URI是每一个资源的独一无二的识别符。

### 2、表现层Respresentation

“资源”所呈现出来的形式，叫做它的“表现层”。文本可以用txt格式呈现，也可以使用HTML、XML、JSON等各式；图片同样有JPG、PNG、BMP等多种表现形式。

URI只代表资源的实体，并不表示其形式。严格地说，有些网址最后的".html"后缀名是不必要的，因为这个后缀名表示格式，属于"表现层"范畴，而URI应该只代表"资源"的位置。它的具体表现形式，应该在HTTP请求的头信息中用Accept和Content-Type字段指定，这两个字段才是对"表现层"的描述。

### 3、状态转移State Transfer

互联网通信协议HTTP协议，是一个无状态协议。这意味着，所有的状态都保存在服务器端。因此，**如果客户端想要操作服务器，必须通过某种手段，让服务器端发生"状态转化"（State Transfer）。而这种转化是建立在表现层之上的，所以就是"表现层状态转化"。**

客户端用到的手段，只能是HTTP协议。具体来说，就是HTTP协议里面，表示操作方式的动词，包括GET、POST、PUT、PATCH、DELETE等。

## 二、API设计指南

### 1、版本

应该将API的版本号放入URL。https://api.example.com/v1/，也可以将版本号放在HTTP头信息中，但是并不直观。

### 2、路径

路径又称"终点"（endpoint），表示API的具体网址。

在RESTful架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以API中的名词也应该使用复数。

例如：

```bash
https://api.moneydodo.com/v1/users   # 正确
https://api.moneydodo.com/v1/getUser # 错误
```

**避免路径过长：**

`/`在url中表达层级，用于**按实体关联关系进行对象导航**，一般根据id导航。

过深的导航容易导致url膨胀，不易维护，如 `GET /users/1/tasks/3/comments/4`，尽量使用查询参数代替路径中的实体导航，如`GET /comments/4?users=1&tasks=3`；

### 3、过滤信息

如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。

下面是一些常见的参数。

```
?limit=10：指定返回记录的每页的数量
?offset=10：指定返回记录的开始位置
?page=2：指定多少页
?orderby=+id：指定返回结果按照哪个属性排序，以及排序顺序
```

参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复。比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。

### 4、HTTP动词

```
GET（SELECT）：从服务器取出资源（一项或多项）。
POST（CREATE）：在服务器新建一个资源。
PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
DELETE（DELETE）：从服务器删除资源。
```

例如：

```
GET     https://api.moneydodo.com/v1/users   
DELETE     https://api.moneydodo.com/v1/users/1
POST     https://api.moneydodo.com/v1/tasks   
PUT     https://api.moneydodo.com/v1/tasks/12
PATCH     https://api.moneydodo.com/v1/users/32
```

还有两个动词：**HEAD：**获取资源的元数据、**OPTIONS：**获取信息，关于资源的哪些属性是客户端可以改变的

### 5、Headers

很多REST API犯的比较大的一个问题是：不怎么理会request headers。对于REST API，有一些HTTP headers很重要：

- Accept：服务器需要返回什么样的content。如果客户端要求返回"application/xml"，服务器端只能返回"application/json"，那么最好返回status code 406 not acceptable（RFC2616），当然，返回application/json也并不违背RFC的定义。一个合格的REST API需要根据Accept头来灵活返回合适的数据。
- If-Modified-Since/If-None-Match：如果客户端提供某个条件，那么当这条件满足时，才返回数据，否则返回304 not modified。比如客户端已经缓存了某个数据，它只是想看看有没有新的数据时，会用这两个header之一，服务器如果不理不睬，依旧做足全套功课，返回200 ok，那就既不专业，也不高效了。
- If-Match：在对某个资源做PUT/PATCH/DELETE操作时，服务器应该要求客户端提供If-Match头，只有客户端提供的Etag与服务器对应资源的Etag一致，才进行操作，否则返回412 precondition failed。

### 6、状态码Status Codes

服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）。

> - 200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
> - 201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
> - 202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
> - 204 NO CONTENT - [DELETE]：用户删除数据成功。
> - 400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
> - 401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
> - 403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
> - 404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
> - 406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
> - 410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
> - 422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
> - 500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

完整的状态码，点击[这里](https://httpstatuses.com/)

### 7、错误处理

1. 不要发生了错误但给2xx响应，客户端可能会缓存成功的http请求；
2. 正确设置http状态码，不要自定义；
3. Response body 提供 1) 错误的代码（日志/问题追查）；2) 错误的描述文本（展示给用户）。

对第三点的实现稍微多说一点：

Java 服务器端一般用异常表示 RESTful API 的错误。API 可能抛出两类异常：业务异常和非业务异常。**业务异常**由自己的业务代码抛出，表示一个用例的前置条件不满足、业务规则冲突等，比如参数校验不通过、权限校验失败。**非业务类异常**表示不在预期内的问题，通常由类库、框架抛出，或由于自己的代码逻辑错误导致，比如数据库连接失败、空指针异常、除0错误等等。

业务类异常必须提供2种信息：

1. 如果抛出该类异常，HTTP 响应状态码应该设成什么；
2. 异常的文本描述；

在Controller层使用统一的异常拦截器：

1. 设置 HTTP 响应状态码：对业务类异常，用它指定的 HTTP code；对非业务类异常，统一500；
2. Response Body 的错误码：异常类名
3. Response Body 的错误描述：对业务类异常，用它指定的错误文本；对非业务类异常，线上可以统一文案如“服务器端错误，请稍后再试”，开发或测试环境中用异常的 stacktrace，服务器端提供该行为的开关。

### 8、返回结果

针对不同操作，服务器向用户返回的结果应该符合以下规范。

```
GET /tasks：返回资源对象的列表（数组）
GET /tasks/taskId：返回单个资源对象
POST /tasks：返回新生成的资源对象
PUT /tasks/taskId：返回完整的资源对象
PATCH /tasks/taskId：返回完整的资源对象
DELETE /tasks/taskId：返回一个空文档
```

### 9、Hypermedia API

RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。

比如，当用户向`api.moneydodo.com`的根目录发出请求，会得到这样一个文档。

```
{"link": {
  "rel":   "collection https://api.moneydodo.com/tasks",
  "href":  "https://api.moneydodo.com/tasks",
  "title": "List of tasks",
  "type":  "application/json"
}}
```

上面代码表示，文档中有一个link属性，用户读取这个属性就知道下一步该调用什么API了。rel表示这个API与当前网址的关系（collection关系，并给出该collection的网址），href表示API的路径，title表示API的标题，type表示返回类型。

Hypermedia API的设计被称为[HATEOAS](http://en.wikipedia.org/wiki/HATEOAS)。Github的API就是这种设计，访问[api.github.com](https://api.github.com/)会得到一个所有可用API的网址列表。

```
{
  "current_user_url": "https://api.github.com/user",
  "authorizations_url": "https://api.github.com/authorizations",
  // ...
}
```

从上面可以看到，如果想获取当前用户的信息，应该去访问[api.github.com/user](https://api.github.com/user)，然后就得到了下面结果。

```
{
  "message": "Requires authentication",
  "documentation_url": "https://developer.github.com/v3"
}
```

上面代码表示，服务器给出了提示信息，以及文档的网址。

### 10、安全性

REST API承前启后，是系统暴露给外界的接口，所以，其安全性非常重要。安全并单单不意味着加密解密，而是一致性（integrity），机密性（confidentiality）和可用性（availibility）。

**请求数据验证**

我们从数据流入REST API的第一步 —— 请求数据的验证 —— 来保证安全性。你可以把请求数据验证看成一个巨大的漏斗，把不必要的访问统统过滤在第一线：

- Request headers是否合法：如果出现了某些不该有的头，或者某些必须包含的头没有出现或者内容不合法，根据其错误类型一律返回4xx。比如说你的API需要某个特殊的私有头（e.g. X-Request-ID），那么凡是没有这个头的请求一律拒绝。这可以防止各类漫无目的的webot或crawler的请求，节省服务器的开销。
- Request URI和Request body是否合法：如果请求带有了不该有的数据，或者某些必须包含的数据没有出现或内容不合法，一律返回4xx。比如说，API只允许querystring中含有query，那么"?sort=desc"这样的请求需要直接被拒绝。有不少攻击会在querystring和request body里做文章，最好的对应策略是，过滤所有含有不该出现的数据的请求。

**数据完整性验证**

REST API往往需要对backend的数据进行修改。修改是个很可怕的操作，我们既要保证正常的服务请求能够正确处理，还需要防止各种潜在的攻击，如replay。数据完整性验证的底线是：保证要修改的数据和服务器里的数据是一致的 —— 这是通过Etag来完成。

Etag可以认为是某个资源的一个唯一的版本号。当客户端请求某个资源时，该资源的Etag一同被返回，而当客户端需要修改该资源时，需要通过"If-Match"头来提供这个Etag。服务器检查客户端提供的Etag是否和服务器同一资源的Etag相同，如果相同，才进行修改，否则返回412 precondition failed。

使用Etag可以防止错误更新。比如A拿到了Resource X的Etag X1，B也拿到了Resource X的Etag X1。B对X做了修改，修改后系统生成的新的Etag是X2。这时A也想更新X，由于A持有旧的Etag，服务器拒绝更新，直至A重新获取了X后才能正常更新。

Etag类似一把锁，是数据完整性的最重要的一道保障。Etag能把绝大多数integrity的问题扼杀在摇篮中，当然，race condition还是存在的：如果B的修改还未进入数据库，而A的修改请求正好通过了Etag的验证时，依然存在一致性问题。这就需要在数据库写入时做一致性写入的前置检查。

### 11、HTTPS

请求的数据和服务器返回的响应都是明文传输，对某些要求比较高的API来说，需要部署HTTPS，在其之上加一层屏障。

### 12、其他

一个实现良好的REST API还应该有如下功能：

- rate limiting：访问限制。
- metrics：服务器应该收集每个请求的访问时间，到达时间，处理时间，latency，便于了解API的性能和客户端的访问分布，以便更好地优化性能和应对突发请求。
- docs：丰富的接口文档 - API的调用者需要详尽的文档来正确调用API，可以用swagger来实现。
- hooks/event propogation：其他系统能够比较方便地与该API集成。比如说添加了某资源后，通过kafka或者rabbitMQ向外界暴露某个消息，相应的subscribers可以进行必要的处理。不过要注意的是，hooks/event propogation可能会破坏REST API的幂等性，需要小心使用。
