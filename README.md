## 部署步骤

1. 克隆项目
```shell
git clone https://github.com/xiangsx/gpt4free-ts-deploy.git
```

2.  配置环境变量

- 把`env`文件夹下面的示例文件`*.env.example`环境变量文件统统去除`.example`后缀
- 你基本只需要修改 gpt4free.env，其他配置不知道具体含义，请勿修改

3. 配置代理
有些站点需要服务器配置代理，方便切换节点，例如`sincode` `perplexity`

国内机器: 配置`clash/config.yaml`, 然后去除`env/proxy.env`中的`http_proxy`字段前的`#`,
注意把你的clash配置复制到 `clash/config.yaml`, 并且确认`external-controller: '0.0.0.0:9090'`, 该字段是监听的 `0.0.0.0`

国外机器: 不需要任何改动

4. 启动

VIP群用户需要先运行`docker login`然后输入群公告的用户名密码

```shell
docker-compose up -d
```

5. 配置oneapi渠道，地址 `http://127.0.0.1:29000`

原封不动按照，截图填入即可,健全密钥随便填就可以了，例如xxx

![oneapi_cfg_help](https://github.com/xiangsx/gpt4free-ts-deploy/assets/29322721/da1676cb-458b-49a7-94ab-e964b8e96514)

映射示例
```json
{
  "gpt-3.5-turbo-0301": "gpt-3.5-turbo",
  "gpt-4-0314": "gpt-4",
  "gpt-4-0613": "gpt-4",
  "gpt-3.5-turbo-0613": "gpt-3.5-turbo"
}
```

6. 【可选】配置监控

配置http监控, 画框的地方，原样填入即可，不用修改
![image](https://github.com/xiangsx/gpt4free-ts-deploy/assets/29322721/82052e78-9bb8-498b-9460-93b8cb2cfcf0)
复制下面的值，填入参数
```json
{
    "site": "auto",
    "model": "gpt-3.5-turbo",
    "prompt": "say 1"
}
```

## 相关链接

1. oneapi配置：http://127.0.0.1:29000
登录用户名密码 root 123456

2. clash配置界面：http://127.0.0.1:29002

3. 监控页面：http://127.0.0.1:29004

## Q&A

1. 出现`ERR PROXY COMMECTION FALED`
出现下图相关错误，检查你的代理，
**解决办法**: 如果你的机器是国外的，去除proxy.env的http_proxy字段或者在前面加个#注释掉; 如果你的机器是国内的，配置代理 `clash/config.yaml`或者使用你自己的代理端口，在proxy.env中修改
![image](https://github.com/xiangsx/gpt4free-ts-deploy/assets/29322721/59d5b369-b856-4d41-84af-bd27c2c45654)

## Release History
v0.0.125-private
1. 更新perplexity提示词，每个人必须自己设置提示词，如果一直超时，说明被封了，就换提示词，
config.perplexity.system 例如 You are a AI model ,base on %s model
(%s 表示model)

v0.0.124-private
1. 忽略

v0.0.123-private
1. auto站点支持转发openai function call

v0.0.122-private
1. 使用新框架重构perplexity，自动处理cf

v0.0.121-private
1. 添加MailTM邮箱，merlin可以用，config.json配置mailtm

v0.0.120-private
1. 修复auto站点，通配符匹配错误的问题

v0.0.119-private
1. 没什么好更新的，修复下merlin，邮箱填emailnator
2. 增加通用联网逻辑，调用auto站点，config.json种需要配置search和url模型, 调用任何其他模型参数body中传search=true

v0.0.118-private
1. 修复poe&poef

v0.0.117-private
1. merlin请求错误，自动销毁重新登录

v0.0.116-private
1. 优化smail-pro,现在用来注册merlin应该没问题了

v0.0.115-private
1. 修复一些莫名其妙的问题

v0.0.114-private
1. 修复代理选择问题

v0.0.113-private
1. 优化smail-pro, 减少超时错误

v0.0.112-private
1. 优化smail-pro注册速度

v0.0.111-private
1. merlin改为串行注册

v0.0.100-private
1. 修复代理选择问题

v0.0.109-private
1. 重构账号池基础架构，支持动态调整poolsize
2. 新增merlin站点，支持gpt4(6000token)和gpt3.5(2500token)

v0.0.108-private
1. 增加API_KEY=xxx参数 加密请求

v0.0.107-private
1. 增加vanus站点，参数VANUS_POOL_SIZE=3

v0.0.106-private
1. myshell网站更新了，修复注册问题
2. myshell站点，单条消息如果太长不报错，随机删减字符

v0.0.105-private
1. 修复myshell注册问题

v0.0.104-private
1. 修复auto站点权重选择的问题

v0.0.103-private
1. auto的config.json配置，增加官方接口格式的第三方负载均衡，可以用来构造集群并配置权重

v0.0.102-private
1. 增加bing和ddg搜索接口
2. www增加max_tokens参数

v0.0.101-private
1. google和www懒加载
2. 修复非stream模式下的一些小问题

v0.0.100-private
1. 增加www站点，用来解析url成文本 site=www&model=url

v0.0.99-private
1. 增加google搜索，site=google&model=search

v0.0.98-private
1. 修复myshell

v0.0.97-private
1. 增加日志logstash配置, 可以把日志通过logstash传到es

v0.0.96-private
1. 修复上个版本崩溃的问题

v0.0.95-private
1. 优化myshell, 增加ws断线重连

v0.0.94-private
1. 增加myshell站点，只需要配置MYSHELL_POOL_SIZE即可，会自动注册，不要滥用奥，少搞点

v0.0.93-private
1. poe和poef增加code llama 三个模型/supports看具体model

v0.0.92-private
1. 修复poef相关问题

v0.0.91-private
1. 重构代码，错误返回结构保持和openai一致
2. perplexity 过cf，目前测试阶段，本地测试一点问题没有，服务器上我这边有问题，发出来大家一起测一下

v0.0.90-private
1. 修复poef

v0.0.89-private
1. 修复poe

v0.0.88-private
1. 增加openai官方逆向3.5接口

v0.0.87-private
1. 增加环境变量PORT 修改容器端口

v0.0.86-private
1. 优化sincode，sincode现在限制对话session数目，优化对话完成删除历史对话

v0.0.85-private
1. 修复日志显示不全的问题

v0.0.84-private
1. 修复per
2. 优化cpu占用，大幅减少io次数

v0.0.83-private
1. 增加auto站点最大重试次数配置

v0.0.82-private
1. 修复perplexity

v0.0.81-private
1. 优化poef站点，自动注册poe账号, 不过目前注册只能是串行的

v0.0.80-private
1. 优化poe

v0.0.79-private
1. sincode最终版，增加随机休眠时间，防止同一时间过期，全部重启导致全部超时

v0.0.78-private
1. sincode终极优化
2. 优化docker构建流程

v0.0.77-private
1. 究极优化sincode，应该不会出现全死的情况了
2. 增加官方openai站点，环境变量OPENAI_KEY=sk-xxxx|sk-xxxx

v0.0.76-private
1. 修复perplexity
2. 优化perplexity，支持gpt-3.5-turbo和net-gpt-3.5-turbo, 取决于你账号的gpt4开关是否打开

v0.0.75-private
1. 更新修复poe

v0.0.74-private
1. 优化sincode，ratelimit 不等待直接重新登录

v0.0.73-private
1. 优化sincode，出现任何异常直接销毁重新登录

v0.0.72-private
1. 修复sincode的部分崩溃问题，需要持续观察，各位先更新

v0.0.71-private
1. 修复this.pool错误

v0.0.70-private
1. per优化内存占用
2. 修复this.pool错误
3. 增加日志文件开关 LOG_FILE=0 #0-关 1-开

v0.0.69-private
1. 增加日志文件，日志控制台打印开关
v0.0.68-private
1. 尽最大可能降低内存占用，目前已优化poe&sincode
v0.0.67-private
1. 修复非流式出现please try later的bug
v0.0.66-private
1. 修复sincode的一系列bug
v0.0.65-private
1. 新增sincode站点


