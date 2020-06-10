### 博客地址
> <a href="https://www.arcinbj.com" target="_blank">在码圈</a>

### 原理

> <a href="https://www.huorong.cn/" target="_blank">Hash一致性闭环算法 - ( 适用于Redis扩容、Nginx多级缓存 等等 )</a>

### Lua模拟脚本

#### 1. 配置文件
```
---
--- Generated by EmmyLua(https://github.com/EmmyLua)
--- Created by parker.
--- DateTime: 2020/5/6 11:19 下午
--- 配置类
---
server = {}

server.address = {
    "192.0.0.1",
    "192.0.0.2",
    "192.0.0.3",
    "192.0.0.4",
    "192.0.0.5"
}

return server

```

#### 2. Hash一致性闭环分发算法
```
---
--- Generated by EmmyLua(https://github.com/EmmyLua)
--- Created by parker.
--- DateTime: 2020/5/6 11:17 下午
--- 纯Lua模拟出oenrestry+lua下Hash一致性闭环代理分发
---
local hashRequestInfo = {}

--- 初始化
hashRequestInfo.init = function()
    local that = hashRequestInfo

    --- 获得配置服务器地址
    local serverArr = require("config.ServerLocal")

    --- 保存服务器数组
    that.ipAddress = serverArr.address

    --- 模拟执行 用户访问
    that.requestMode()
end

--- 获得服务器索引 取模
hashRequestInfo.getServerIndex = function(args)
    local that = hashRequestInfo
    local count = #that.ipAddress
    local index = args % count + 1
    return index
end

--- 获得对应服务器映射地址
hashRequestInfo.getServerAddress = function(args)
    local that = hashRequestInfo
    local index = that.getServerIndex(args)
    local serverIpAddress = that.ipAddress[index]
    return serverIpAddress
end

--- 模拟执行
hashRequestInfo.requestMode = function()
    local that = hashRequestInfo
    
    --- 模拟 1000 个 用户访问
    for i = 1, 1000 do
        local randomNum = math.random(1000,100000)
        local serverAddress = that.getServerAddress(randomNum)
        print("访问服务器地址：http://"..serverAddress)
    end

end

--- 执行
hashRequestInfo.init()

```
