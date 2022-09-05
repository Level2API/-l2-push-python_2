Level2行情  QQ：2037696191  Skype：Level2.API  
联系邮箱：goLevel2API@gmail.com

### 订阅主题
名称格式`证券交易所_证券代码_证券行情消息类型(bit或值)`，如：

- `2_000002_1` 订阅深交所股票`000002`的`逐笔成交`推送消息
- `1_600005_3` 订阅上交所股票`600005`的`逐笔成交和逐笔委托(1|2==3)`推送消息
- 订阅个股的L2逐笔成交和L2逐笔委托数据

## 数据字典

### 响应码

| 值   | 说明       |
| ---- | ---------- |
| 0    | 未知       |
| 1    | OK         |
| 2    | 非法参数   |
| 3    | 非法状态   |
| 4    | 内部错误   |
| 5    | 未登录     |
| 6    | 禁止操作   |
| 7    | 超时       |
| 8    | 请求太频繁 |

### 用户角色

| 值   | 说明         |
| ---- | ------------ |
| 0    | 未知         |
| 1    | 普通用户     |

### 证券交易所

| 值   | 说明   |
| ---- | ------ |
| 0    | 未知   |
| 1    | 上交所 |
| 2    | 深交所 |
| 3    | 北交所 |

### 证券行情消息类型

| 值   | 说明         |
| ---- | ------------ |
| 0    | 未知         |
| 1    | 逐笔成交     |
| 2    | 逐笔委托     |
| 4    | 委托队列     |
| 8    | 股票十档行情 |

## 基本说明

接入流程：
- 开通用户账号
- 下载行情工具，修改配置文件，启动本地代理服务器
- 编写代码，调用本地代理服务器实现的GRPC接口，接收行情消息推送


行情工具[请点击下载](https://github.com/Level2API/l2-push-python/tree/master/cli)，目录说明：

| 名称       | 说明               |
| ---------- | ------------------ |
| conf       | 配置目录           |
| data       | 数据目录           |
| log        | 日志目录           |
| txtool     | 命令行工具-Linux   |
| txtool.exe | 命令行工具-Windows |



行情工具常用命令：

| 命令             | 说明                                                |
| ---------------- | --------------------------------------------------- |
| `txtool -h`      | 查看帮助                                            |
| `txtool version` | 查看工具版本号                                      |
| `txtool proxy`   | 启动本地代理服务器。默认命令，Windows可直接双击执行 |




行情接入示例：

- [Python](https://github.com/Level2API/l2-push-python)



## 接入说明（Go项目为例）

1. 下载行情工具，修改`conf/proxy.toml`
   - 设置用户名和密码：`User/Passwd`
   - 设置推送服务器地址：`RpcServer/TcpServer`
2. 打开命令窗口，切换到`cli`目录，执行命令启动本地代理服务器
   - Linux系统执行：`txtool proxy`
   - Windows系统执行：`txtool.exe proxy`
   - 如果提示本机端口已占用，可修改配置项`Address(代理服务器监听地址)`
3. 启动成功，调用代理服务器提供的GRPC接口
   - 接口地址见配置项`Address`，默认为：`localhost:8090`
   - 接口定义，见目录`proto`
   - Go项目已生成接口代码，见目录`rpc`
   - Go项目接口调用示例，见`main_test.go`
4. 代理服务器配置
   - `proxy.toml` 配置代理服务器监听地址，是否将推送消息写入本地文件等
   - `log.toml` 配置日志格式，是否写入控制台和文件等


## 订阅说明
用户可订阅行情消息主题以便接收推送：
- 全订阅用户不支持新增/删除订阅主题
- 目前用户订阅数只计算已订阅主题的证券代码数
- 如果配置将推送消息写入文件，建议至少保证50G可用磁盘空间，并及时压缩和清理旧数据文件



订阅主题名称格式`证券交易所_证券代码_证券行情消息类型(bit或值)`，如：

- `2_000002_1` 订阅深交所股票`000002`的`逐笔成交`推送消息
- `1_600005_3` 订阅上交所股票`600005`的`逐笔成交和逐笔委托(1|2==3)`推送消息
