# 接口前缀

http://xxxxxxxx:yyyy

所有接口返回格式都是json：
```
{
 "status":true,
 "msg":"SUCCESS",
 "data":...
}
```

其中`status`字段表示后台是否处理成功；`msg`字段表示返回的提示信息；`data`字段
属性为可变结构，根据返回数据不同可能结构不同，根据各接口返回实际数据为准；

所有请求需在头部加上`LANG`字段，将当前浏览器语言缩写转大写后带入，如`LANG:EN`或`LANG:ZH-CN`

> 正常请求`status`不为true时直接弹出一个错误提示框，提示内容直接显示`msg`字段里内容即可；
> 如果返回的状态码是401则跳到登录页面

# 通用接口

主要包括注册登录和获取底部4个按钮的文字描述

## 注册

POST `/register`  json

```
{
 "user_name":"theName",   
 "pwd":"password",
 "trx_addr":"ssssdddddddddddddd",
 "parent_id":12
}
```
parent_id为可选项，如果用户是通过分享页进入到注册页面，则提取当前url里的`invite`参数
作为parent_id的预填值

返回userInfo和一个token，token要存入localStore，后续每个请求要把token放入head中的`token`字段，
如`token:xxxxxxxxxxxxxxxxxxxxxxxxxxxx.xxxxxxxx.xxxxxxxxxxxxxxxx`

## 登录

POST `/login`  json

```
{
 "user_name":"theName",   
 "pwd":"password"
}
```

返回userInfo和一个token，token要存入localStore，后续每个请求要把token放入head中的`token`字段，
如`token:xxxxxxxxxxxxxxxxxxxxxxxxxxxx.xxxxxxxx.xxxxxxxxxxxxxxxx`

## 获取底部四个按钮名称

GET `/homeDesc`

```
{
 "status":true,
 "msg":"SUCCESS",
 "data":["bt1","bt2","bt3","bt4"]
}
```

# 首页接口

## 获取首页顶部信息

GET `/main/mainInfo`

## 获取banner图片地址

GET `/main/banners`

图片3秒轮播就行

## 获取具体按钮下的详细信息

GET `/main/getGame`

参数名：`name`，参数值就是`/main/mainInfo`接口返回的gList里的name

## 获取首页第一个表格数据

GET `/main/recentLogs`

## 获取首页第二个和第三个表格数据

GET `/main/wae`

返回两个数组，exs是首页第二个表格数据，txcs是首页第三个也就是最后一个表格数据

## 获取首页最后文本条目

GET `/main/contents`

```
{
    "Status": true,
    "Msg": "SUCCESS",
    "Data": [
        {
            "content": [
                "https://www.baidu.com/ico.png",    //这里是具体内容，前面自动给个序号
                "url",
                "https://www.baidu.com",
                "desc",
                "testttttt"
            ],
            "title": "test"      //这个是小节标题，前面带个序号
        },
        {
            "content": [
                "https://www.baidu.com/ico.png",
                "url",
                "https://www.baidu.com",
                "desc",
                "testttttt"
            ],
            "title": "test2" //这个是小节标题，前面带个序号
        },
        {
            "content": [
                "https://www.baidu.com/ico.png",
                "url",
                "https://www.baidu.com",
                "desc",
                "testttttt"
            ],
            "title": "test3"
        }
    ]
}
```

# 记录页接口

## 获取表格信息

GET `/record/logs`

参数page默认0表示第一页，size默认10，只关注created_at，amount，stat和msg三个参数就行，
stat==0的时候msg显示成红色类，其他值都把msg显示成蓝色类

## 获取表格下方两个数值

GET `/record/stats`

```
{
    "Status": true,
    "Msg": "SUCCESS",
    "Data": {
        "a": "14017.22",
        "b": "2002.46"
    }
}
```

a表示左边的值，b是右边的值

# 分享页接口

## 获取分享页信息

GET `/share/info`

```
{
    "Status": true,
    "Msg": "SUCCESS",
    "Data": {
        "a": 1,
        "b": 2,
        "c": 3,
        "d": 4,
        "e": 5,
        "f": 6,
        "g": 7,
        "h": 8,
        "i": 9,
        "link": "https://www.xxxx.xxx/reg?invite=1"
    }
}
```

link表示中间那个分享链接，剩余a-i这9个值分别从左到右，从上到下对应9处变量

## 获取规则页信息

GET `/share/rule`

```
{
    "Status": true,
    "Msg": "SUCCESS",
    "Data": {
        "data": [
            {
                "decimal": "0.008",
                "name": "1"
            },
            {
                "decimal": "0.002",
                "name": "2"
            }
        ],
        "rs": [
            {
                "Title": "rt1",
                "Content": "ctttttttttttttttttt1"
            },
            {
                "Title": "rt2",
                "Content": "ctttttttttttttttt2"
            },
            {
                "Title": "rt3",
                "Content": "ctttttttttttttttttt3"
            }
        ]
    }
}
```

data是表格数据，name是第一列，decimal是第二列；

rs是表格下面的文字信息，Title表示标题，Content是该标题下的正文内容

# “我的”接口

这部分主要是三个表格接口，其他几个按钮，如分享页就跳分享页面，退出就删除local Store里的token后跳到首页

三个接口都是get，都是两个可选参数page（默认0）和size（默认10）

> /my/rLog

> /my/cLog

> /my/wLog
