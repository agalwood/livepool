# LivePool
Fiddler Like cross platform debugging proxy for web developers base on NodeJS

LivePool 是一个基于 NodeJS，类似 Fiddler 支持抓包和本地替换的 Web 开发调试工具，是 Tencent AlloyTeam 在开发实践过程总结出的一套的便捷的 WorkFlow 以及调试方案。

## 版本
version: 0.7.5

## 特性
- 基于 NodeJS, 跨平台
- 支持 http 抓包和本地替换调试，Https/WebSockets 直接代理转发（暂不支持本地替换）
- 便捷的 UI 管理界面，跟 Fiddler 类似，降低学习成本
- 可以脱离 UI 后台运行，适应于某些不需要抓包，只需要使用替换和简单路由的场景
- 基于项目的替换规则管理，方便高效，规则支持拖曳排序
- 支持基于请求路径的本地文件替换，支持基于请求路径的路由转发（host 配置）
- 替换类型支持：文件/文件夹替换，combo合并替换，qzmin替换（批量combo)，delay延时等
- 支持自动设置系统代理
- 支持规则过滤，只显示关注的请求
- 提供构建 http get/post 请求界面，方便接口调试
- 特色功能：模拟gprs/3g等低网速（mac only）
- 特色功能：支持离线站点到本地，并自动代码格式化

## 安装
- 先安装 nodejs, 参考官网 http://nodejs.org

### 从 git 下载安装
- 下载， 运行 livepool
``` shell
git clone https://github.com/rehorn/livepool
```
- 安装依赖
```shell
cd ~/livepool
npm install
```
- 运行 livepool
```shell
node livepool.js
```

### 使用 npm 进行全局安装
``` shell
npm install livepool -g
```

- 运行 livepool
```shell
livepool
```

### 使用
- 将浏览器的代理设置为 http://127.0.0.1:8090, chrome 可以通过 switchsharp 进行
- 打开浏览器，http://127.0.0.1:8002
- 打开需要调试页面地址，如 http://im.qq.com

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot1.png)

### 界面说明

![界面说明](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot2.png)

1. 菜单区
2. Session（显示所有http请求信息）
3. TreeView（使用树状结构显示Session信息）
4. 功能Tab: Pool（按照项目管理本地替换规则）
5. 功能Tab: Inspector (session查看器，查看请求header，body等信息)
6. 功能Tab: Composer（http请求模拟器，可以模拟http get/post请求）
7. 功能Tab: Filter（session过滤器，根据规则过滤session，只保留关注的）
8. 功能Tab: Log（日志显示）
9. 功能Tab: Timeline（session时间轴，comming soon）
10. 功能Tab: Statics（统计，对站点性能进行评估，comming soon）

## 使用 LivePool 进行抓包
拦截所有的 http 请求，查看分析请求内容
### 设置代理
##### 浏览器代理
手动将浏览器代理设置为 127.0.0.1:8090, Chrome 可使用 SwitchSharp 等插件进行代理切换，这样浏览器发出的所有的请求就能通过 livepool 中抓取
##### 系统全局代理
livepool 可以设置系统全局代理，实现系统所有 http 请求的抓取

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot3.png)

### 在 Session 中浏览请求

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot10.png)

- 灰色背景：命中本地替换规则，并返回了本地内容的请求
- 绿色：js 请求
- 玫红：css 请求
- 蓝色：json 请求
- 黑色：其他类型请求

### 使用 inspector 查看请求内容

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot4.png)

##### http request
可以查看http request的header，cookie等信息

##### http response
可以查看http response的header，cookie，视图等信息

##### 视图切换
可以查看图片、JSON、文本代码，并且对代码进行格式化，快速添加替换规则

tips: 便捷操作
> - 双击 sesssion 区域请求，快速查看请求内容
> - 在 TreeView 中节点，快速滚动到该请求，并查看对应请求内容
> - 右键复制请求 url
> - 右键在浏览器打开该 url
> - 右键 replay，再次发起该请求

## 本地替换开发
将浏览器请求替换为本地文件，进行线上调试或本地开发，修改立刻生效

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot5.png)

### 新建项目
填写项目名称和根目录

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot6.png)

### 新建替换规则
填写handler替换规则或router路由规则

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot7.png)

### 文件替换规则

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot8.png)

##### 延时规则
将请求阻塞指定的时间，再返回给浏览器，可以用来测试极端网络下资源阻塞的页面表现

##### combo规则
将本地的多个文件合并为一个之后，返回给浏览器，多个文件路径之间使用“|”作为间隔符，一般用来开发调试站点js/css资源分模块进行文件存储的情况

##### qzmin规则（批量combo规则）
combo规则批量版本，使用一个json文件指定合并规则，便于替换和管理，文件格式请参考范例，[find.all.qzmin](https://github.com/rehorn/livepool/blob/master/test/examples/tools/find.all.qzmin)

##### 文件替换
将某个请求拦截，并使用本地文件替换，返回浏览器，可以用于本地开发调试

##### 文件夹替换
将指定路径的请求，使用本地文件夹下同名文件进行替换（未找到对应文件则直接代理），返回浏览器，可以用于本地开发调试

> tips: 便捷操作
> - 拖曳规则可以将规则进行快速排序
> - 从 session（界面区域1）拖曳请求到 Pool（界面区域4），可以快速创建本地替换规则
> - 快捷键：shift+c 复制当前选中项目或规则
> - 通过工具栏 export/import 进行规则的导入导出

### 路由规则

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot9.png)

- 使用 ‘-’ 表示直接代理：将请求直接代理转发到目标机器
- ip 路由：将命中的请求路由到指定机器（相当于配置 host）

## 请求构建器
模拟 http 请求，可以修改get/post请求参数

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot11.png)

## 过滤器
使用规则过滤不重要的请求

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot12.png)

## 日志
显示系统信息、错误日志等

## 模拟低网速[mac]
模拟网络质量较差网络，查看站点表现

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot3.png)

- GPRS: 48kbit/s
- Edge: 64kbit/s
- 3g: 348kbit/s
- ADSL: 768kbit/s
- WIFI: 2048kbit/s

## 离线站点到本地
- 将站点内容离线到本地，并自动代码格式化，便于查看
- 站点保存到当前文件夹 Sites 下

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot13.png)

## 快捷键

![效果图](http://raw.github.com/rehorn/livepool/master/test/screenshot/shot14.png)

## TODO
- 完善 Timeline时间轴、Stat统计界面
- LiveReload、AlloyDesinger集成，
- 支持构建工具 task 管理与运行，如 Grunt, Gulp, Mod
- More....

## Thanks
extjs(sencha)
http-proxy
express
socket.io
underscore
