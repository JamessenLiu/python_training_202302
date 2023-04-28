### 概述

- 表现层转态转化
Representational State Transfer
    - 资源（resources）
    REST的名称”表现层状态转化”中，省略了主语。”表现层”其实指的是”资	源”（Resources）的”表现层”。资源指网络上一个具体信息（图片，文	本….），每个资源对应特定的URI。
    - 表现层（representation）
    资源的外在表现形势，如用jpg, png表示图片，HTTP请求头中的accept和	content-type是对表现层的描述。
    - 状态转化（state tansfer）
    HTTP协议无状态，状态都保存在服务器，客户端需要特定方式让服务器产	生状态转化，比如GET，POST，PUT，DELETE
- 每个URI（URL）表示一种资源
    - URI — 统一资源标示符
    - URL - 统一资源定位符
- 客户端和服务器之间，传递资源的表现层
- 客户端通过HTTP的方法，对服务端资源进行操作，实现状态转换
- 基于URI实现对资源的管理及访问，具有扩展性强、结构清晰的特点

### 设计

- 误区
    - URI不要包含动词
    资源是实体，不应该包含动词，动作应该放在HTTP协议中

```
如把某个用户的级别从1调整为2
错误示范
PUT /user/level/1/to/2
正确示范
PUT /user/level?from=1&to=2

```

```
* 不建议在URI中加入版本号
不同的版本，可以理解成同一种资源的不同表现形式，所以应该采用同一	个URI，版本号可以在HTTP请求头信息的Accept字段中进行区分

```

```
错误示范
/api/v1/user
正确示范
/api/user
Accept：version=1

```

- HTTP动词 + URI宾语
restful的核心是”动词 + 宾语“结构，如

```
GET /users/1
POST /companies

```

动词通常是5中HTTP方法

```
	GET：读取（Read）
	POST：新建（Create）
	PUT：更新（Update）
	PATCH：更新（Update），通常是部分更新
	DELETE：删除（Delete）

```

- 宾语为名词

```
错误示范
/getALlUser
/createUser

```

- 单数和复数
建议使用复数，获取单个资源通过ID区分
```
GET /users
GET /users/1

```
