# 服务端开发文档

欢迎你，钉钉微应用的开发者，我们很期待你成为钉钉微应用的开发者；

微应用是 钉钉 为连接企业办公打造的移动入口，通过微应用你可以将企业的业务审批，内部系统，生成，协作，管理，上下游沟通连接到钉钉，更简单和低成本实现企业移动化； 结合钉钉的基础通信能力，让企业应用更活跃，员工更高效，移动化成本更低。

此文档很适合于:

1. 企业的IT部，了解钉钉如何连接你所在企业的办公；

2. 软件开发服务商，了解如何通过钉钉为您的客户提供定制的企业办公软件，提升你的服务价值。

## 建立连接

你可以使用以下三种方式，将钉钉微应用连接到你的企业应用：

1. 企业应用服务器调用钉钉开放平台提供的接口，以钉钉微应用的身份给企业用户的钉钉账号推送消息，以下称 **主动调用模式**。

2. 钉钉用户在使用企业提供的微应用H5页面时，该页面可以调用钉钉提供的原生接口，使用钉钉开放的终端能力，以下称 **JSAPI模式**。

### 主动调用

当企业应用服务器调用钉钉开放平台接口时，需使用https协议、Json数据格式、UTF8编码，访问域名为 https://oapi.dingtalk.com。
在每次主动调用钉钉开放平台接口时需要带上AccessToken参数。AccessToken参数由CorpID和CorpSecret换取。

CorpID是企业在钉钉中的标识，每个企业拥有一个唯一的CorpID；

CorpSecret是企业每个应用的凭证密钥。

CorpID及CorpSecret可以在钉钉为企业提供的管理后台中找到，由钉钉自动分配。

<aside class="notice">
POST请求请在HTTP Header中设置 Content-Type:application/json，否则接口调用失败
</aside>

#### 获取AccessToken

AccessToken是企业访问钉钉开放平台接口的全局唯一票据，调用接口时需携带AccessToken。

AccessToken需要用CorpID和CorpSecret来换取，不同的CorpSecret会返回不同的AccessToken。正常情况下AccessToken有效期为7200秒，有效期内重复获取返回相同结果，并自动续期。

###### 请求说明

Https请求方式: GET
`https://oapi.dingtalk.com/gettoken?corpid=id&corpsecret=secrect`

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
corpid | String | 是 | 企业Id
corpsecret | String | 是 | 企业应用的凭证密钥

###### 返回说明

a)正确的Json返回结果:

```
{
    "errcode": 0,
    "errmsg": "ok",
    "access_token": "fw8ef8we8f76e6f7s8df8s"
}
```

参数 | 说明
---- | -----
errcode | 错误码
errmsg | 错误信息
access_token | 获取到的凭证

b)错误的Json返回示例:

```
{
   "errcode": 43003,
   "errmsg": "require https"
}
```

## 管理通讯录

### 获取部门列表

###### 请求说明

Https请求方式: GET

`https://oapi.dingtalk.com/department/list?access_token=ACCESS_TOKEN`

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token | String | 是 | 调用接口凭证

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "ok",
    "department": [
        {
           "id": 2,
            "name": "来往事业部",
            "parentid": 1
        },
        {
            "id": 3,
            "name": "服务端开发组",
            "parentid": 2
        }
    ]
}
```

参数 | 说明
---- | -----
errcode | 返回码
errmsg | 对返回码的文本描述内容
department | 部门列表数据。以部门的order字段从小到大排列
id | 部门id
name | 部门名称
parentid | 父部门id，根部门为1

### 创建部门

###### 请求说明

Https请求方式: POST

`https://oapi.dingtalk.com/department/create?access_token=ACCESS_TOKEN`

###### 请求包结构体

```
{
    "name": "来往事业部",
    "parentid": "1",
    "order": "1"
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
----------| ------- | ------- | ------
access_token | String | 是 | 调用接口凭证
name | String | 是 |  部门名称。长度限制为1~64个字符
parentid | String | 是 | 父部门id。根部门id为1
order  | String | 否 | 在父部门中的次序值。order值小的排序靠前

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "created",
    "id": 2
}
```

参数 | 说明
----------  | ------
errcode | 返回码
errmsg | 对返回码的文本描述内容
id | 创建的部门id


### 更新部门

###### 请求说明

Https请求方式: POST

`https://oapi.dingtalk.com/department/update?access_token=ACCESS_TOKEN`

###### 请求包结构体

```
{
    "name": "来往事业部",
    "parentid": "1",
    "order": "1",
    "id": "1"
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
----------| -------  | ------- | ------
access_token | string | 是 | 调用接口凭证
name | String | 否 | 部门名称。长度限制为1~64个字符
parentid | String | 否 | 父部门id。根部门id为1
order | String | 否 | 在父部门中的次序值。order值小的排序靠前
id | long | 是 | 部门ID

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "updated"
}
```

参数 | 说明
---------- | ------
errcode | 返回码
errmsg | 对返回码的文本描述内容

### 删除部门

###### 请求说明

Https请求方式: GET

`https://oapi.dingtalk.com/department/delete?access_token=ACCESS_TOKEN&id=ID`

###### 参数说明

参数 | 参数类型 | 必须 | 说明
----------| -------  | ------- | ------
access_token | String | 是 | 调用接口凭证
id | long | 是 | 部门ID。（注：不能删除根部门；不能删除含有子部门、成员的部门）

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "deleted"
}
```

参数 | 说明
---------- | ------
errcode | 返回码
errmsg | 对返回码的文本描述内容

### 获取成员

###### 请求说明

Https请求方式: GET

`https://oapi.dingtalk.com/user/get?access_token=ACCESS_TOKEN&userid=zhangsan`

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token | String | 是 | 调用接口凭证
userid | String |是 | 员工在企业内的UserID，企业用来唯一标识用户的字段。

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "ok",
    "userid": "zhangsan",
    "name": "张三",
    "department": [1, 2],
    "position": "工程师",
    "avatar": "dingtalk.com/abc.jpg",
    "extattr": {
        "attrs": [
            {
                "name":"爱好",
                "value":"旅游"
            },
            {
                "name":"卡号",
                "value":"1234567234"
            }
        ]
    }
}
```

参数 |说明
---------- | ------
errcode | 返回码
errmsg | 对返回码的文本描述内容
userid | 员工UserID。对应管理端的帐号
name | 成员名称
department | 成员所属部门id列表
position | 职位信息
avatar | 头像url
extattr | 扩展属性

### 创建成员

###### 请求说明

Https请求方式: POST

`https://oapi.dingtalk.com/user/create?access_token=ACCESS_TOKEN`

###### 请求包结构体

```
{
    "userid": "zhangsan",
    "name": "张三",
    "department": [1, 2],
    "position": "产品经理",
    "mobile": "15913215421",
    "email": "zhangsan@gzdev.com",
    "extattr": {
        "attrs": [
            {
                "name":"爱好",
                "value":"旅游"
            },
            {
                "name":"卡号",
                "value":"1234567234"
            }
        ]
    }
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | -------  | ------- | ------
access_token |String | 是 | 调用接口凭证
userid | String| 是 | 员工UserID。对应管理端的帐号，企业内必须唯一。长度为1~64个字符
name | String | 是 | 成员名称。长度为1~64个字符
department | List | 否 |数组类型，数组里面值为整型，成员所属部门id列表
position |String | 否 | 职位信息。长度为0~64个字符
mobile | String| 是 | 手机号码。企业内必须唯一
email | String| 否 | 邮箱。长度为0~64个字符。企业内必须唯一
extattr | JSONObject | 否 | 扩展属性

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "created"
}
```

参数 | 说明
---------- | ------
errcode | 返回码
errmsg | 对返回码的文本描述内容

### 更新成员

###### 请求说明

Https请求方式: POST

`https://oapi.dingtalk.com/user/update?access_token=ACCESS_TOKEN`

###### 请求包结构体

```
{
    "userid": "zhangsan",
    "name": "张三",
    "department": [1, 2],
    "position": "产品经理",
    "mobile": "15913215421",
    "email": "zhangsan@gzdev.com",
    "extattr": {
        "attrs": [
            {
                "name":"爱好",
                "value":"旅游"
            },
            {
                "name":"卡号",
                "value":"1234567234"
            }
        ]
    }
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token |String | 是 | 调用接口凭证
userid |String | 是 | 员工UserID。对应管理端的帐号，企业内必须唯一。长度为1~64个字符
name |String | 是 | 成员名称。长度为1~64个字符
department |List | 否 | 成员所属部门id列表
position | String| 否 | 职位信息。长度为0~64个字符
mobile |String | 是 | 手机号码。企业内必须唯一
email |String | 否 | 邮箱。长度为0~64个字符。企业内必须唯一
extattr |JSONObject | 否 | 扩展属性

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "updated"
}
```

参数 | 说明
---------- | ------
errcode | 返回码
errmsg | 对返回码的文本描述内容

### 删除成员

###### 请求说明

Https请求方式: GET

`https://oapi.dingtalk.com/user/delete?access_token=ACCESS_TOKEN&userid=ID`


###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token | String | 是 | 调用接口凭证
userid | String | 是 | 员工UserID。对应管理端的帐号

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "deleted"
}
```

参数 | 说明
---------- | ------
errcode | 返回码
errmsg | 对返回码的文本描述内容

### 批量删除成员

###### 请求说明
Https请求方式: POST

`https://oapi.dingtalk.com/user/batchdelete?access_token=ACCESS_TOKEN`

###### 请求包结构
```
{
   "useridlist":["zhangsan","lisi"]
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
----------| -------  | ------- | ------
access_token | String| 是 | 调用接口凭证
useridlist | List | 是 | 员工UserID列表。对应管理端的帐号，列表长度在1到20之间

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "deleted"
}
```

参数 | 说明
---------- | ------
errcode | 返回码
errmsg | 对返回码的文本描述内容

### 获取部门成员

###### 请求说明

Https请求方式: GET

`https://oapi.dingtalk.com/user/simplelist?access_token=ACCESS_TOKEN&department_id=1&fetch_child=0`

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token | String | 是 | 调用接口凭证
department_id | long | 是 | 获取的部门id

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "ok",
    "userlist": [
        {
            "userid": "zhangsan",
            "name": "张三"
            "active": true,
        }
    ]
}
```

参数 | 说明
---------- | ------
errcode | 返回码
errmsg | 对返回码的文本描述内容
userlist | 成员列表
userid | 员工UserID,对应管理端的帐号
name | 成员名称
active | 表示该用户是否激活了钉钉

### 获取部门成员（详情）

###### 请求说明

Https请求方式: GET

`https://oapi.dingtalk.com/user/list?access_token=ACCESS_TOKEN&department_id=1&fetch_child=0`

参数 | 参数类型 | 必须 | 说明
---------- | -------  | ------- | ------
access_token |String | 是 | 调用接口凭证
department_id | long | 是 | 获取的部门id

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "ok",
    "userlist":[
        {
            "userid": "zhangsan",
            "name": "张三",
            "active": true,
            "department": [1, 2],
            "position": "工程师",
            "email": "zhangsan@alibaba-inc.com",
            "openId": "HJHJYT*Y8887",
            "avatar":  "./dingtalk/abc.jpg",
            "status": 1,
            "extattr": {
                "attrs": [
                    {
                        "name":"爱好",
                        "value":"旅游"
                    },
                    {
                        "name":"卡号",
                        "value":"1234567234"
                    }
                ]
            }
        }
    ]
}
```

参数 | 说明
---------- | ------
errcode | 返回码
errmsg | 对返回码的文本描述内容
userlist | 成员列表
userid | 员工UserID,对应管理端的帐号
name | 成员名称
active | 表示该用户是否激活了钉钉
department | 成员所属部门id列表
position |  职位信息
email | 邮箱
openId | 开放平台openId
avatar  | 头像url
extattr |  扩展属性

<!--
"mobile": "15913215421",
mobile | 手机号码
-->

<!--## 发送普通会话消息

员工可以把消息发送到同企业的人或群。

调用接口时，使用Https协议、JSON数据包格式。

目前支持文本、图片、语音、普通文件、OA消息以及link等消息类型，每个消息都由消息头和消息体组成，普通会话的消息头由sender,cid组成。消息体请参见以下各种[消息类型](#消息类型及数据格式)。

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
sender | String | 是 | 消息发送者员工ID
cid | String | 是 | 群消息或者个人聊天会话Id（通过JSAPI唤起联系人界面拿到会话ID）

普通会话消息样例：

![msg1](https://img.alicdn.com/tps/TB1FVMvIFXXXXcnXXXXXXXXXXXX.jpg)

### 发送普通会话消息接口说明

###### 请求说明

Https请求方式: POST

`https://oapi.dingtalk.com/message/send_to_conversation?access_token=ACCESS_TOKEN`

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token |String | 是 | 调用接口凭证

###### 返回说明

如果是群，返回跟发送者同一家企业的一组工号；如果是个人聊天，只返回发送者同一家企业的一个工号；不在同一家企业，发送失败

```
{
    "errcode": 0,
    "errmsg": "ok",
    "receiver": "UserID1|UserID2"
}
```
-->
## 发送企业会话消息

企业可以主动发消息给员工，消息量不受限制。

调用接口时，使用Https协议、JSON数据包格式。

目前支持text、image、voice、file、link、OA消息类型。每个消息都由消息头和消息体组成，企业会话的消息头由touser,toparty,agentid组成。消息体请参见以下各种[消息类型](#消息类型及数据格式)。

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
touser |String | 否 | 员工ID列表（消息接收者，多个接收者用' &#124; '分隔）。特殊情况：指定为@all，则向该企业应用的全部成员发送
toparty |String | 否 | 部门ID列表，多个接收者用' &#124; '分隔。当touser为@all时忽略本参数
agentid | String | 是 |企业应用id，这个值代表以哪个应用的名义发送消息，如果不填，则以当前获取accesstoken的应用作为发消息的应用

企业会话消息样例：

![msg2](https://img.alicdn.com/tps/TB1PAQwIFXXXXXOXXXXXXXXXXXX.jpg)

### 发送企业消息接口说明

###### 请求说明

Https请求方式: POST

`https://oapi.dingtalk.com/message/send?access_token=ACCESS_TOKEN`

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token |String | 是 | 调用接口凭证

###### 返回说明

如果收件人、部门或标签不存在，发送仍然执行，但返回无效的部分。

```
{
    "errcode": 0,
    "errmsg": "ok",
    "invaliduser": "UserID1|UserID2",
    "invalidparty":"PartyID1"
}
```

## 消息类型及数据格式

### text消息

```
{
    "msgtype": "text",
    "text": {
        "content": "张三的请假申请"
    }
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
msgtype |String | 否 | 消息类型，此时固定为：text
content |String | 是 | 消息内容

### image消息

```
{
    "msgtype": "image",
    "image": {
        "media_id": "MEDIA_ID"
    }
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
msgtype |String | 否 | 消息类型，此时固定为：image
media_id | String | 是 | 图片媒体文件id，可以调用上传媒体文件接口获取。建议宽600像素 x 400像素，宽高比3：2

### voice消息

```
{
    "msgtype": "voice",
    "voice": {
       "media_id": "MEDIA_ID"
    }
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
msgtype |String | 否 | 消息类型，此时固定为：text
media_id |String | 是 | 语音媒体文件id，可以调用上传媒体文件接口获取。2MB，播放长度不超过60s，AMR格式

### file消息

```
{
    "msgtype": "file",
    "file": {
       "media_id": "MEDIA_ID"
    }
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
msgtype | String| 否 | 消息类型，此时固定为：file
media_id |String | 是 | 媒体文件id，可以调用上传媒体文件接口获取。10MB


### link消息

```
{
    "msgtype": "link",
    "link": {
        "messageUrl": "http://s.dingtalk.com/market/dingtalk/error_code.php",
        "picUrl":"@lALOACZwe2Rk",
        "title": "测试",
        "text": "测试"
    }
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
msgtype | String | 是 | 消息类型，此时固定为：link

##### link消息体格式

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
link.messageUrl | String | 是 | 消息点击链接地址
link.picUrl | String | 是 | 图片媒体文件id，可以调用上传媒体文件接口获取
link.title | String | 是 | 消息标题
link.text | String | 是 | 消息描述

### OA消息

```
{
     "msgtype": "oa",
     "oa": {
        "message_url": "http://dingtalk.com",
        "head": {
            "bgcolor": "FFBBBBBB",
            "text": "头部标题"
        },
        "body": {
            "title": "正文标题",
            "form": [
                {
                    "key": "姓名:",
                    "value": "张三"
                },
                {
                    "key": "年龄:",
                    "value": "20"
                },
                {
                    "key": "身高:",
                    "value": "1.8米"
                },
                {
                    "key": "体重:",
                    "value": "130斤"
                },
                {
                    "key": "学历:",
                    "value": "本科"
                },
                {
                    "key": "爱好:",
                    "value": "打球、听音乐"
                }
            ],
            "rich": {
                "num": "15.6",
                "unit": "元"
            },
            "content": "大段文本大段文本大段文本大段文本大段文本大段文本大段文本大段文本大段文本大段文本大段文本大段文本",
            "image": "@lADOADmaWMzazQKA",
            "file_count": "3",
            "author": "李四 "
        }
    }
}
```

###### 参数说明

参数 | 参数类型 | 必须 | 说明
----- | ------- | ------- | ------
msgtype |String | 否 | 消息类型，此时固定为：oa

#### OA消息体内容

###### 参数说明

参数 | 参数类型 | 必须 | 说明
------ | ------- | ------- | ------
oa.message_url| String | 是 | 客户端点击消息时跳转到的H5地址
oa.head | String | 是 | 消息头部内容
oa.head.bgcolor | String | 是 | 消息头部的背景颜色。长度限制为8个英文字符，其中前2为表示透明度，后6位表示颜色值。不要添加0x
oa.head.text | String | 是 | 消息的头部标题
oa.body | Array[JSON Object] | 是 | 消息体
oa.body.title | String | 否 | 消息体的标题
oa.body.form | Array[JSON Object] | 否 | 消息体的表单，最多显示6个，超过会被隐藏
oa.body.form.key | String | 否 | 消息体的关键字
oa.body.form.value | String | 否 | 消息体的关键字对应的值
oa.body.rich | Array[JSON Object] | 否 | 单行富文本信息
oa.body.rich.num | String | 否 | 单行富文本信息的数目
oa.body.rich.unit | String | 否| 单行富文本信息的单位
oa.body.content | String | 否 | 消息体的内容，最多显示3行
oa.body.image | String | 否 | 消息体中的图片media_id
oa.body.file_count | String | 否 | 自定义的附件数目。此数字仅供显示，钉钉不作验证
oa.body.author | String | 否 | 自定义的作者名字

#### OA消息截图

![oames](https://img.alicdn.com/tps/TB1gVcFIFXXXXcGXXXXXXXXXXXX.jpg)

## 管理多媒体文件

企业在使用接口时，对多媒体文件、多媒体消息的获取和调用等操作，是通过media_id来进行的。通过本接口，企业可以上传或下载多媒体文件。

### 上传媒体文件

用于上传图片、语音等媒体资源文件以及普通文件（如doc，ppt），接口返回媒体资源标识ID：media_id。请注意，media_id是可复用的，同一个media_id可用于消息的多次发送。

###### 请求说明

Https请求方式: POST

`https://oapi.dingtalk.com/media/upload?access_token=ACCESS_TOKEN&type=TYPE`

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token | String | 是 | 调用接口凭证
type |String | 是 | 媒体文件类型，分别有图片（image）、语音（voice）、普通文件(file)
media |String | 是 | form-data中媒体文件标识，有filename、filelength、content-type等信息

###### 返回结果

```
{
    "errcode": 0,
    "errmsg": "ok",
    "type": "image",
    "media_id": "@dsa8d87y7c8d8c"
}
```

参数 |说明
---------- | ------
errcode | 错误码
errmsg | 错误信息
type | 媒体文件类型，分别有图片（image）、语音（voice）、普通文件(file)
media_id | 媒体文件上传后获取的唯一标识
created_at | 媒体文件上传时间戳

###### 上传的媒体文件限制

* 图片（image）:1MB，支持JPG格式
* 语音（voice）：2MB，播放长度不超过60s，AMR格式
* 普通文件（file）：10MB

### 获取媒体文件

通过media_id获取图片、语音等文件。

###### 请求说明

Https请求方式: GET

`https://oapi.dingtalk.com/media/get?access_token=ACCESS_TOKEN&media_id=MEDIA_ID`

###### 参数说明

参数 |参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token |String | 是 | 调用接口凭证
media |String | 是 | form-data中媒体文件标识，有filename、filelength、content-type等信息

###### 返回说明

和普通的http下载相同，请根据http头做相应的处理。

```
  HTTP/1.1 200: OK
  Connection: close
  Content-Type: image/jpeg
  Content-disposition: attachment; filename="MEDIA_ID.jpg"
  Date: Sun, 04 Jan 2015 12:00:00 GMT
  Cache-Control: no-cache, must-revalidate
  Content-Length: 1234567
  ...
```

a)正确时返回：

```
{
    "errcode": 40004,
    "errmsg": "invalid media_id"
}
```

b)错误时返回（这里省略了HTTP首部）：

## 免登服务

通过免登服务，在企业微应用的H5中，可以获取当前使用钉钉的用户身份及相关信息。

通过此接口获取用户身份会有一定的时间开销。对于频繁获取用户身份的场景，建议采用如下方案：

1. 用户跳转到企业页面时，企业校验是否有代表用户身份的cookie，此cookie由企业生成
2. 如果没有获取到cookie，调用下述服务，获取用户身份后，由企业生成代表用户身份的cookie
3. 根据cookie获取用户身份，进入相应的页面

### 使用标准OAUTH2.0

使用标准OAUTH2.0 HTTP 302 跳转方式获取CODE

在钉钉用户访问企业的H5时，如果企业应用服务器未从cookie中发现用户信息，则需要构造一个连接，通过HTTP 302跳转方式，让钉钉用户访问钉钉开放平台授权网址，构造的地址如下:

`https://oapi.dingtalk.com/connect/oauth2/authorize?appid=CORPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE`

钉钉开放平台授权服务器在判断当前用户属于当前企业时，会再次通过HTTP 302跳转回企业应用的H5，并在H5的URL后面追加参数code=CODE&state=STATE

<aside class="notice">
有安全限制，REDIRECT_URI域名必须包含在企业所有的微应用域名内,微应用请到钉钉oa后台->选择微应用菜单->微应用中心，添加微应用
</aside>

例如：企业拥有www.taobao.com和www.tianmao.com两个域名，如果跳转到www.abc.com则会提示无权限访问。

###### 参数说明

参数 | 参数类型 | 必须 | 说明
----------| ------- | ------- | ------
appid | String | 是 | 企业的CorpID
redirect_uri | String | 是 | 授权后重定向的回调链接地址，请使用urlencode对链接进行处理
response_type | String | 是 | 返回类型，此时固定为：code
scope | String | 是 | 应用授权作用域，此时固定为：snsapi_base
state | String | 是 | 重定向后会带上state参数，企业可以填写a-zA-Z0-9的参数值，长度不可超过128个字节
\#ding_redirect | | 是 | 钉钉终端使用此参数判断是否需要带上身份信息

举例如下，假如企业的H5地址是 `http://www.abc.com/oa`

企业的CorpID是ding12345678

企业自定义的state是abcd1234

当钉钉用户在访问微应用内的H5时，企业自有服务器发现cookie中无法读取到用户信息时，需要构造如下链接并进行HTTP 302跳转

`https://oapi.dingtalk.com/connect/oauth2/authorize?appid=ding12345678&redirect_uri=http%3a%2f%2fwww.abc.com%2foa&response_type=code&scope=snsapi_base&state=abcd1234`

假如点击链接的当前钉钉用户属于当前企业(ding12345678)，则会HTTP 302跳到如下url。如果当前钉钉用户不属于当前企业，页面提示无权访问。

`http://www.abc.com/oa?code=CODE&state=abcd1234`

### 通过CODE换取用户身份

企业应用的服务器在拿到CODE后，需要将CODE发送到钉钉开放平台接口，如果验证通过，则返回CODE对应的用户信息。

###### 请求说明

Https请求方式: GET

`https://oapi.dingtalk.com/user/getuserinfo?access_token=ACCESS_TOKEN&code=CODE`

###### 参数说明

参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token | String | 是 | 调用接口凭证
code | String | 是 | 通过Oauth认证会给URL带上CODE

###### 返回结果

正确时返回示例如下：

```
{
    "errcode": 40029,
    "errmsg": "invalid code",
    "userid": "USERID",
    "deviceId":"DEVICEID"
}
```

参数 | 说明
---------- | ------
userId | 员工在企业内的UserID
deviceId | 手机设备号,由钉钉在安装时随机产生

出错时返回示例如下：

```
{
    "errcode": 40029,
    "errmsg": "invalid code"
}
```

## JS接口API

### 获取jsapi_ticket

企业在使用微应用中的JS API时，需要先从钉钉开放平台接口获取jsapi_ticket生成签名数据，并将最终签名用的部分字段及签名结果返回到H5中，JS API底层将通过这些数据判断H5是否有权限使用JS API。

###### 请求说明
Https请求方式：GET

`https://oapi.dingtalk.com/get_jsapi_ticket?access_token=ACCESS_TOKE`

###### 参数说明
参数 | 参数类型 | 必须 | 说明
---------- | ------- | ------- | ------
access_token | String | 是 | 调用接口凭证
type | String | 是 | 这里是固定值，jsapi

######  返回结果
正确时返回示例如下：

```
{
    "errcode": 0,
    "errmsg": "ok",
    "ticket": "dsf8sdf87sd7f87sd8v8ds0vs09dvu09sd8vy87dsv87",
    "expires_in": 7200
}
```

参数 | 说明
---------- | ------
errcode | 错误码
errmsg | 错误信息
ticket | 用于JS API的临时票据
expires_in | 票据过期时间

出错时返回示例如下：

```
{
    "errcode": 45009,
    "errmsg": "接口调用超过限制"
}
```

### JS-SDK

JS-SDK 为H5页面提供了一系列调用原生用的UI控件或者服务的JS接口，文档地址如下：

[http://open.dingtalk.com/#客户端开发文档](http://open.dingtalk.com/#客户端开发文档)

## 附录
### 全局返回码说明

开发者每次调用接口时，可能获得正确或错误的返回码，企业可以根据返回码信息调试接口，排查错误。

###### 全局返回码说明如下：

参数 | 说明
 ---- | -----
-1 | 系统繁忙
0 | 请求成功
40001 | 获取access_token时Secret错误，或者access_token无效
40002 | 不合法的凭证类型
40003 | 不合法的UserID
40004 | 不合法的媒体文件类型
40005 | 不合法的文件类型
40006 | 不合法的文件大小
40007 | 不合法的媒体文件id
40008 | 不合法的消息类型
40009 | 不合法的部门id
40010 | 不合法的父部门id
40011 | 不合法的排序order
40012 | 不合法的发送者
40013 | 不合法的corpid
40014 | 不合法的access_token
40015 | 发送者不在会话中
40016 | 不合法的会话ID
40017 | 在会话中没有找到与发送者在同一企业的人
40029 | 不合法的oauth_code
40031 | 不合法的UserID列表
40032 | 不合法的UserID列表长度
40033 | 不合法的请求字符，不能包含uxxxx格式的字符
40035 | 不合法的参数
40038 | 不合法的请求格式
40039 | 不合法的URL长度
40048 | url中包含不合法domain
40055 | 不合法的agent结构
40056 | 不合法的agentid
40057 | 不合法的callbackurl
40061 | 设置应用头像失败
40062 | 不合法的应用模式
40064 | 管理组名字已存在
40065 | 不合法的管理组名字长度
40066 | 不合法的部门列表
40067 | 标题长度不合法
40073 | 不合法的openid
40077 | 不合法的预授权码
40078 | 不合法的临时授权码
40079 | 不合法的授权信息
40080 | 不合法的suitesecret
40082 | 不合法的suitetoken
40083 | 不合法的suiteid
40084 | 不合法的永久授权码
40085 | 不合法的suiteticket
40086 | 不合法的第三方应用appid
40086 | 不合法的第三方应用appid
40087 | 创建永久授权码失败
40088 | 不合法的套件key或secret
41001 | 缺少access_token参数
41002 | 缺少corpid参数
41003 | 缺少refresh_token参数
41004 | 缺少secret参数
41005 | 缺少多媒体文件数据
41006 | 缺少media_id参数
41008 | 缺少oauth
41009 | 缺少UserID
41010 | 缺少url
41011 | 缺少agentid
41012 | 缺少应用头像mediaid
41013 | 缺少应用名字
41014 | 缺少应用描述
41015 | 缺少Content
41016 | 缺少标题
41021 | 缺少suiteid
41022 | 缺少suitetoken
41023 | 缺少suiteticket
41024 | 缺少suitesecret
41025 | 缺少永久授权码
41026 | 需要tmp_auth_code
41027 | 需要授权企业的corpid参数
41030 | 企业未对该套件授权
41031 | auth_corpid和permanent_code不匹配
42001 | access_token超时
42002 | refresh_token超时
42003 | oauth_code超时
42007 | 预授权码失效
42008 | 临时授权码失效
42009 | suitetoken失效
43001 | 需要GET请求
43002 | 需要POST请求
43003 | 需要HTTPS
43004 | 无效的HTTP HEADER Content-Type
43005 | 需要Content-Type为application/json;charset=UTF-8
43007 | 需要授权
43010 | 需要处于回调模式
43011 | 需要企业授权
44001 | 多媒体文件为空
44002 | POST的数据包为空
44003 | 图文消息内容为空
44004 | 文本消息内容为空
45001 | 多媒体文件大小超过限制
45002 | 消息内容超过限制
45003 | 标题字段超过限制
45004 | 描述字段超过限制
45005 | 链接字段超过限制
45006 | 图片链接字段超过限制
45007 | 语音播放时间超过限制
45008 | 图文消息超过限制
45009 | 接口调用超过限制
45016 | 系统分组，不允许修改
45017 | 分组名字过长
45018 | 分组数量超过上限
45024 | 账号数量超过上限
46001 | 不存在媒体数据
46004 | 不存在的员工
47001 | 解析JSON/XML内容错误
48002 | API禁用
48003 | suitetoken无效
48004 | 授权关系无效
50001 | redirect_uri未授权
50002 | 员工不在权限范围
50003 | 应用已停用
50005 | 企业已禁用
60001 | 部门长度不符合限制
60002 | 部门层级深度超过限制
60003 | 部门不存在
60004 | 父亲部门不存在
60005 | 不允许删除有成员的部门
60006 | 不允许删除有子部门的部门
60007 | 不允许删除根部门
60008 | 部门名称已存在
60009 | 部门名称含有非法字符
60010 | 部门存在循环关系
60011 | 管理员权限不足，（user/department/agent）无权限
60012 | 不允许删除默认应用
60013 | 不允许关闭应用
60014 | 不允许开启应用
60015 | 不允许修改默认应用可见范围
60017 | 不允许设置企业
60018 | 不允许更新根部门
60020 | 访问ip不在白名单之中
60102 | UserID已存在
60103 | 手机号码不合法
60104 | 手机号码已存在
60105 | 邮箱不合法
60106 | 邮箱已存在
60110 | 部门个数超出限制
60111 | UserID不存在
60112 | 成员姓名不合法
60113 | 身份认证信息（手机/邮箱）不能同时为空
60114 | 性别不合法
60118 | 用户无有效邀请字段（邮箱，手机号）
60119 | 不合法的position
60120 | 用户已禁用
60121 | 找不到该用户
60122 | 不合法的extattr
70001 | 企业不存在或者已经被解散
70002 | 获取套件下的微应用失败
70003 | agentid对应微应用不存在
70004 | 企业下没有对应该agentid的微应用
70005 | ISV激活套件失败
80001 | 可信域名没有IPC备案，后续将不能在该域名下正常使用jssdk
900001 | 加密明文文本非法
900002 | 加密时间戳参数非法
900003 | 加密随机字符串参数非法
900004 | 不合法的aeskey
900005 | 签名不匹配
900006 | 计算签名错误
900007 | 计算加密文字错误
900008 | 计算解密文字错误
900009 | 计算解密文字长度不匹配
900010 | 计算解密文字corpid不匹配

## Demo

提供了使用Java、PHP、Nodejs 接入钉钉开放平台API的代码示例

下面的代码示例展示了一些常用API的使用方式，在运行下面的示例前请先获取CorpID和CorpSecret，并按照示例下的说明配置到合适的位置。

### Java版本

```javascript
 public class Env {
  public static final String OAPI_HOST = "https://oapi.dingtalk.com";
	public static final String CORP_ID = "corpid";
	public static final String SECRET = "secret";
 }

```

1.将您的CorpID和CorpSecret配置在Env.java文件

2.启动您的服务器，如果配置正确，则会成功启动。

[Demo地址：](https://github.com/injekt/openapi-demo-java)
`https://github.com/injekt/openapi-demo-java`

### PHP版本

```javascript
 define("OAPI_HOST", "https://oapi.dingtalk.com");
 define("CORPID", "");
 define("SECRET", "");
```

1.将您的CorpID和CorpSecret配置在env.php文件

2.启动您的服务器，如果配置正确，则会成功启动。

[Demo地址：](https://github.com/injekt/openapi-demo-php)
`https://github.com/injekt/openapi-demo-php`

### Node.js版本

```javascript
module.exports = {
    corpId: '',
    secret: ''
};
```

1.将您的CorpID和CorpSecret配置在env.js文件

2.启动您的服务器，如果配置正确，则会成功启动。

[Demo地址：](https://github.com/injekt/openapi-demo-nodejs)
`https://github.com/injekt/openapi-demo-nodejs`
