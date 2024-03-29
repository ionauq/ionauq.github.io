---
title: "压测工具"
date: 2020-10-28T09:00:00+08:00
tags: ["压测工具"]
categories: ["工具使用"]
---

命令行下常用的压测工具有`ab`和`wrk`。


ab
======

```Shell
yum install httpd-tools
```


wrk
======

[wrk](https://github.com/wg/wrk) 是一款 C 语言开发的压测工具，并且支持使用LuaJIT定制化请求。


```Shell
wrk -t12 -c400 -d30s http://127.0.0.1:8080/index.html
```


```
使用方法: wrk <选项> <HTTP URL>                            
  Options:                                            
    -c, --connections <N>  跟服务器建立并保持的TCP连接数量  (Connections to keep open)  
    -d, --duration    <T>  压测时间                      (Duration of test)        
    -t, --threads     <N>  使用多少个线程进行压测           (Number of threads to use)
                                                    
    -s, --script      <S>  指定Lua脚本路径                 (Load Lua script file)      
    -H, --header      <H>  为每一个HTTP请求添加HTTP头       (Add header to request)         
        --latency          在压测结束后，打印延迟统计信息     (Print latency statistics)
        --timeout     <T>  Socket/请求超时时间             (Socket/request timeout)       
    -v, --version          wrk版本信息                    (Print version details)
                                                      
  <N>代表数字参数，支持国际单位 (1k, 1M, 1G)
  <T>代表时间参数，支持时间单位 (2s, 2m, 2h)
```

### lua 函数

wrk支持在启动阶段、运行阶段和结束阶段三个阶段对请求进行定制化处理。

##### 启动阶段
```
function setup(thread)
```
##### 运行阶段
```
function init(args)

function delay()

function request()

function response(status, headers, body)

```

##### 结束阶段

```
function done(summary, latency, requests)
```

### 使用方式示例

指定线程数、TCP连接数、压测时间并使用lua脚本

```
wrk -t50 -c500 -d30s --latency  -s load.lua http://localhost/filter
```

load.lua

```
wrk.method = "POST"
wrk.headers["Content-Type"] = "application/json"


key_length = 16

local function get_random_key(n) 
    local t = {
        "0","1","2","3","4","5","6","7","8","9",
        "a","b","c","d","e","f","g","h","i","j","k","l","m","n","o","p","q","r","s","t","u","v","w","x","y","z",
        "A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z",
    }    
    local s = ""
    for i =1, n do
        s = s .. t[math.random(#t)]        
    end;
    return s
end;

local function create_keys()
    local keys = {}
    for i=1, 100 do
      keys[i] = get_random_key(key_length)
    end;
    return keys
end;

local function get_body()
    keys= create_keys()
    body = {
        filter="bloom",
        keyspace="default",
        action=2,
        keys = keys,
    }

    local cjson = require "cjson"
    str_body = cjson.encode(body)
    return str_body
end;


function request()
    wrk.body = get_body()
    return wrk.format()
end;
```
