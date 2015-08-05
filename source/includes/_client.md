# 客户端开发文档

## 通用

### 页面引入js文件

`http://g.alicdn.com/ilw/ding/0.3.8/scripts/dingtalk.js`

或者

`https://g.alicdn.com/ilw/ding/0.3.8/scripts/dingtalk.js`

### 全局变量、命名空间

直接引入dingtalk.js会得到一个全局变量`dd`，支持amd、cmd引入方式

全局变量dd，命名空间：设备(dd.device)、业务(dd.biz)

### 权限验证配置

钉钉JS API安全验证，只有经过安全验证的微应用才能调用安全级别较高的API。该功能在钉钉客户端iOS版2.2.2和安卓版2.2.0开始支持。

获取JS-API权限验证使用的签名的方法请参考附录-[JS-API权限签名算法](#js-api权限签名算法)

```javascript
 dd.config({
    appId: '', // 必填，微应用ID
    corpId: '',//必填，企业ID
    timestamp: , // 必填，生成签名的时间戳
    nonceStr: '', // 必填，生成签名的随机串
    signature: '', // 必填，签名
    jsApiList: ['device.notification.alert', 'device.notification.confirm'] // 必填，需要使用的jsapi列表
});
```

##### 参数说明

参数 | 参数类型 | 必须 | 说明
------ | ----- | ------| ------
appId | String |是 | 微应用ID
corpId | String |是 | 企业ID
timestamp | String |是 | 生成签名的时间戳
nonceStr | String |是 | 生成签名的随机串
signature | String |是 | 签名
jsApiList | Array | 是 | 需要调用的jsapi列表

### 通过ready接口处理成功验证

须把相关接口放在ready函数中调用来确保正确执行。

config信息验证后会执行ready方法，所有接口调用都必须在config接口获得结果之后，config是一个客户端的异步操作，所以如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行。对于用户触发时才调用的接口，则可以直接调用，不需要放在ready函数中。

```javascript
dd.ready(function(){
    ;
});
```

### 通过error接口处理失败验证

config信息验证失败会执行error函数，错误信息可以在返回的error参数中参看

```javascript
dd.error(function(error){
    ;
});
```

### 接口约定

* 所有接口都为异步
* 接受一个object类型的参数
* 成功回调 onSuccess
* 失败回调 onFail

```javascript
dd.命名空间.功能.方法({
    参数1: '',
    参数2: '',
    onSuccess: function(result) {
    //成功回调
    /*
    {
        //所有返回信息都输出在这里
    }*/
    },
    onFail: function(){
    //失败回调
    }
})
```

## 容器

dd.runtime

### 获取容器信息

```javascript
dd.runtime.info({
    onSuccess: function(result) {
    /*{
        ability: '0.0.2' //容器版本，用来标识JSAPI能力，可根据该版本来决定能否使用jsapi
    }*/
    }
})
```

##### 返回说明

参数 | 说明
---- | -----
ability | 容器版本，用来标识JSAPI能力，可根据该版本来决定能否使用jsapi

### 授权

### 请求授权码

0.0.5

```javascript
dd.runtime.permission.requestAuthCode({
    onSuccess: function(result) {
    /*{
        code: 'hYLK98jkf0m' //string authCode
    }*/
    }
})
```

##### 返回说明

参数 | 说明
---- | -----
code | 授权码


<!--### 请求jsapi

0.0.5

```javascript
dd.runtime.permission.requestJsApis({
    corpId: '', //企业ID
    timeStamp: '', //生成签名的时间戳
    nonceStr: '', //生成签名的随机串
    signature: '', //签名
    jsApiList: [], //需要调用的jsapi列表
    onSuccess: function(result) {
    /*{
        jsApilist: [] //array[string] authJsApiList
    }*/
    }
})
```
##### 参数说明

参数 | 参数类型 | 必须 | 说明
----- | ------- | ------- | ------
corpId | String | 是 | 企业ID
timeStamp | String | 是 | 生成签名的时间戳
nonceStr | String | 是 | 生成签名的随机串
signature | String | 是 | 签名
jsApiList | array[string] | 是 | 需要调用的jsapi列表

##### 返回说明

参数 | 说明
---- | -----
jsApilist | 授权的JsApi列表
-->

## 设备

dd.device

### 基础信息

### 获取设备唯一识别码

0.0.5

```javascript
dd.device.base.getUUID({
    onSuccess : function(data) {
        /*
        {
            uuid: '3udbhg98ddlljokkkl' //
        }
        */
    },
    onFail : function(err) {}
});
```

#####　返回说明

参数 | 说明
---- | -----
uuid | 设备唯一识别码

### 获取热点接入信息

0.0.5

```javascript
dd.device.base.getInterface({
    onSuccess : function(data) {
        /*
        {
            ssid: 'alibaba-inc',
            macIp: '3c:12:aa:09'
        }
        */
    },
    onFail : function(err) {}
});
```

##### 返回说明

参数 | 说明
---- | -----
ssid | 热点ssid
macIp | 热点mac地址

## 启动器

dd.device

### 检测应用是否安装

0.0.5

```javascript
dd.device.launcher.checkInstalledApps({
    apps: ['taobao', 'tmall'], //应用scheme
    onSuccess : function(data) {
        /*
        {
            installed: ['taobao', 'tmall'] //应用scheme
        }
        */
    },
    onFail : function(err) {}
});
```

##### 参数说明

参数 | 参数类型 | 说明
----- | ------- | ------
apps | Array[String] | 应用scheme

##### 返回说明

参数 | 说明
---- | -----
installed | 应用scheme应用scheme

### 启动第三方应用

0.0.5

```javascript
dd.device.launcher.launchApp({
    app: 'taobao', //应用scheme
    onSuccess : function(data) {
        /*
        {
            result: true //true 唤起成功 false 唤起失败
        }
        */
    },
    onFail : function(err) {}
});
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ------- | -------
app | String | 应用scheme

##### 返回说明

参数 | 说明
---- | -----
result | 值为true说明唤起成功，值为false说明唤起失败


### 获取当前网络类型

0.0.2

```javascript
dd.device.connection.getNetworkType({
    onSuccess : function(data) {
        /*
        {
            result: 'wifi' // result值: wifi 2g 3g 4g unknown none   none表示离线
        }
        */
    },
    onFail : function(err) {}
});
```
##### 返回说明

参数 | 说明
---- | -----
result | result值: wifi、2g、3g、4g、unknown、none，none表示离线


### 事件

支持事件： pause、resume、online、offline、backbutton(android)
0.0.2 全部支持

swipeRefresh 0.0.5

```
//退到后台(webview)
document.addEventListener('pause', function(e) {
    e.preventDefault();
    console.log('事件：pause')
}, false);

//唤醒(webview)
document.addEventListener('resume', function(e) {
    e.preventDefault();
    console.log('事件：resume')
}, false);

//在线
document.addEventListener('online', function(e) {
    e.preventDefault();
    console.log('事件：online')
}, false);

//离线
document.addEventListener('offline', function(e) {
    e.preventDefault();
    console.log('事件：offline')
}, false);

//返回按钮(android)
document.addEventListener('backbutton', function(e) {
    e.preventDefault();
    dd.device.notification.alert({
        message: '哎呀，你不小心点到返回键啦!',
        title: '...警告...'
    });
}, false);

//webview下拉刷新
document.addEventListener('swipeRefresh', function(e) {
    e.preventDefault();
    console.log('事件：swipeRefresh')
}, false);
```

## 弹窗

dd.device

### alert

```javascript
dd.device.notification.alert({
    message: String,
    title: String,//可传空
    buttonName: String,
    onSuccess : function() {
        /*回调*/
    },
    onFail : function(err) {}
});
```

##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
message | String | 消息内容
title | String | 弹窗标题
buttonName | String |按钮名称


### confirm

```javascript
dd.device.notification.confirm({
    message: String,
    title: String,
    buttonLabels: ['确认', '取消'],
    onSuccess : function(result) {
        /*
        {
            buttonIndex: 0 //被点击按钮的索引值，Number类型，从0开始
        }
        */
    },
    onFail : function(err) {}
});
```

##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
message | String | 消息说明
title | String |标题
buttonLabels | Array[String]| 按钮名称

##### 返回说明

参数 | 说明
---- | -----
buttonIndex | 被点击按钮的索引值，Number类型，从0开始

### prompt

```javascript
dd.device.notification.prompt({
    message: String,
    title: String,
    buttonLabels: ['确认', '取消'],
    onSuccess : function(result) {
        /*
        {
            buttonIndex: 0, //被点击按钮的索引值，Number类型，从0开始
            value: '' //输入的值
        }
        */
    },
    onFail : function(err) {}
});
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
message | String |消息内容
title | String |标题
buttonLabels | Array[String] | 按钮名称

##### 返回说明

参数 | 说明
---- | -----
buttonIndex | 被点击按钮的索引值，Number类型，从0开始
value | 输入的值

### vibrate

```javascript
dd.device.notification.vibrate({
    duration: 300, //震动时间，android可配置 iOS忽略
    onSuccess : function(result) {
        /*
        {}
        */
    },
    onFail : function(err) {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
duration | Number | 震动时间，android可配置 iOS忽略

### showPreloader

```javascript
dd.device.notification.showPreloader({
    text: String, //loading显示的字符，空表示不显示文字
    showIcon: Boolean, //是否显示icon，默认true
    onSuccess : function(result) {
        /*{}*/
    },
    onFail : function(err) {}
})
```

##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
text | String | loading显示的字符，空表示不显示文字
showIcon | Boolean | 是否显示icon，默认true

### hidePreloader

```javascript
dd.device.notification.hidePreloader({
    onSuccess : function(result) {
        /*{}*/
    },
    onFail : function(err) {}
})
```

### toast

```javascript
dd.device.notification.toast({
    icon: '', //icon样式，有success和error，默认为空 0.0.2
    text: String, //提示信息
    duration: Number, //显示持续时间，单位秒，默认按系统规范[android只有两种(<=2s >2s)]
    delay: Number, //延迟显示，单位秒，默认0
    onSuccess : function(result) {
        /*{}*/
    },
    onFail : function(err) {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
icon | Boolean | icon样式，有success和error，默认为空 0.0.2
text | String | 提示信息
duration |  Number |显示持续时间，单位秒，默认按系统规范[android只有两种(<=2s >2s)]
delay | Number | 延迟显示，单位秒，默认0


### actionsheet

0.0.2

```javascript
dd.device.notification.actionSheet({
    title: '', //标题
    cancelButton: '取消', //取消按钮文本
    otherButtons: ['按钮1', '按钮2', '按钮3'],
    onSuccess : function(result) {
        /*{
            buttonIndex: 0 //被点击按钮的索引值，Number，从0开始, 取消按钮为-1
        }*/
    },
    onFail : function(err) {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
title | String | 标题
cancelButton | String |取消按钮文本
otherButtons | Array[String] | 其他按钮列表

##### 返回说明

参数 | 说明
----- | -----
buttonIndex | 被点击按钮的索引值，Number，从0开始, 取消按钮为-1


## 地图

dd.device

### 获取当前地址位置

```javascript
dd.device.geolocation.get({
    onSuccess : function(result) {
        /*
        {
            longitude : Number,
            latitude : Number,
            accuracy : Number,
        }
        */
    },
    onFail : function(err) {}
});
```
##### 返回说明

参数 | 说明
----- | -----
longitude | 经度
latitude | 纬度
accuracy  | 准确度

### 搜索
0.0.6

```javascript
dd.biz.map.search({
  scope: Number, //定位范围
  extraInfo: {
	  prompt: "提示信息"
  }
  onSuccess: function(data) {
  /* data结构
	  {
	  "province": ""//省
	  "provinceCode": "" //省份编码
	  "city": "" //城市
	  "cityCode": "" //城市编码
	  "adCode": "" // POI的行政区划代码
	  "adName": "" // POI的行政区划名称
	  "distance": "" //获取POI距离中心点的距离
	  "postCode": "" //返回POI的邮编
	  "title": "" //POI的名称
	  "snippet": "" //POI的地址
	  "longitude": 11.11, //该点经度
	  "latitude": 11.11, //该点纬度
	  }
  */
  },
  onFail : function(err) {
  }
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
scope | Number | 定位范围
extraInfo.prompt | String | 提示信息

##### 返回说明

参数 | 说明
----- | -----
province | 省份
provinceCode | 省份编码
city | 城市
cityCode | 城市编码
adCode | POI的行政区划代码
adName | POI的行政区划名称
distance | 获取POI距离中心点的距离
postCode | 返回POI的邮编
title | POI的名称
snippet | POI的地址
longitude | 该点经度
latitude | 该点纬度

###定位
0.0.6

```javascript
dd.biz.map.locate({
  onSuccess: function(data) {
   /* data结构
	  {
	  "province": ""//省
	  "provinceCode": "" //省份编码
	  "city": "" //城市
	  "cityCode": "" //城市编码
	  "adCode": "" // POI的行政区划代码
	  "adName": "" // POI的行政区划名称
	  "distance": "" //获取POI距离中心点的距离
	  "postCode": "" //返回POI的邮编
	  "title": "" //POI的名称
	  "snippet": "" //POI的地址
	  "longitude": 11.11, //该点经度
	  "latitude": 11.11, //该点纬度
	  }
  */
  },
  onFail : function(err) {
  }
})
```

##### 返回说明

参数 | 说明
----- | -----
province | 省份
provinceCode | 省份编码
city | 城市
cityCode | 城市编码
adCode | POI的行政区划代码
adName | POI的行政区划名称
distance | 获取POI距离中心点的距离
postCode | 返回POI的邮编
title | POI的名称
snippet | POI的地址
longitude | 该点经度
latitude | 该点纬度

## 加速器

dd.device

### 摇一摇

开启监听

```javascript
dd.device.accelerometer.watchShake({
    sensitivity: 20,//振动幅度，Number类型，加速度变化超过这个值后触发shake
    frequency: 150,//采样间隔(毫秒)，Number类型，指每隔多长时间对加速度进行一次采样， 然后对比前后变化，判断是否触发shake
    callbackDelay: 3000,//触发『摇一摇』后的等待时间(毫秒)，Number类型，防止频繁调用
    onSuccess : function(result) {
        /*
        {}
        */
    },
    onFail : function(err) {}
});
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
sensitivity | Number |振动幅度，Number类型，加速度变化超过这个值后触发shake
frequency | Number | 采样间隔(毫秒)，Number类型，指每隔多长时间对加速度进行一次采样， 然后对比前后变化，判断是否触发shake
callbackDelay| Number |触发『摇一摇』后的等待时间(毫秒)，Number类型，防止频繁调用


清除监听

```javascript
dd.device.accelerometer.clearShake({
    onSuccess : function(result) {
        /* 调用成功
        */
    },
    onFail : function(err) {}
});
```


## 业务

dd.biz

### 功能

#### 打开应用内页面

目前支持打开邮箱附件和打开聊天页面，

a.邮箱附件

```
// 页面名称：
    mailAttachment
// 传参：
    mailId: String
    attachmentId: String
    title: String
    size: Number
```

b.个人资料页

```
// 页面名称：
    profile
// 传参：
    id 用户工号 String
    corpId: '' //企业id
```

c.聊天页面

```
// 页面名称：
    chat
// 传参：
    users: ['123'] 用户列表,工号
    corpId: '' //企业id
```

d.免费电话页面

```
// 页面名称：
    call
// 传参：
```

f.联系人添加页面

0.0.4

```
// 页面名称：
    contactAdd
// 传参：
```

g.唤起添加好友页面

0.0.4

```
// 页面名称：
    friendAdd
// 传参：
```

#### 打开远程url

```javascript
dd.biz.util.openLink({
    url: String,
    showMenuBar: true, //是否显示右上角按钮，默认true 显示 0.0.2
    credible: true, //是否可信的url，即白名单机制，可信的url支持调用jsapi，默认true 0.0.2
    enableShare: true, //是否启用默认分享(服务端抓取内容)，如果为false，则分享组件只有刷新动作; 默认true
    onSuccess : function() {
        /**/
    },
    onFail : function(err) {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
url | String |远程url
showMenuBar | Boolean |是否显示右上角按钮，默认true 显示 0.0.2
credible | Boolean |是否可信的url，即白名单机制，可信的url支持调用jsapi，默认true 0.0.2
enableShare | Boolean | 是否启用默认分享(服务端抓取内容)，如果为false，则分享组件只有刷新动作; 默认true

#### 分享

```javascript
dd.biz.util.share({
    type: Number,//分享类型，0:全部组件 默认； 1:只能分享到钉钉；2:不能分享，只有刷新按钮
    url: String,
    title: String,
    content: String,
    image: String,
    onSuccess : function() {
        /**/
    },
    onFail : function(err) {}
})
```
##### 参数说明

参数 | 参数类型 |  说明
----- | ----- | -----
type | Number | 分享类型，0:全部组件默认； 1:只能分享到钉钉；2:不能分享，只有刷新按钮
url | String | url地址
title | String | 分享标题
content | String | 分享内容
image | String |分享的图片


#### ut打点

```javascript
dd.biz.util.ut({
    key: String,//打点名
    value: String,//打点传值
    onSuccess : function() {
        /**/
    },
    onFail : function(err) {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
key | String | 打点名
value | String | 打点传值


<!--### 选取图片[暂不开放]

```javascript
dd.biz.util.chooseImage({
    multiple: false, //是否多选，默认false
    destType: String, //返回格式 DATA_URL(base64编码) FILE_URI(文件路径)，默认FILE_URI
    onSuccess : function(result) {
        /**/
    },
    onFail : function() {}
})
```
-->

#### 上传图片
选择图片+上传，防止恶意上传

```javascript
dd.biz.util.uploadImage({
    multiple: false, //是否多选，默认false
    max: 3, //最多可选个数
    onSuccess : function(result) {
        /*
        [
          'http://gtms03.alicdn.com/tps/i3/TB1VF6uGFXXXXalaXXXmh5R_VXX-237-236.png'
        ]
        */
    },
    onFail : function() {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
multiple | Boolean | 是否多选，默认false
max | Number | Number为正整数，最多可选个数



#### 图片浏览器

```javascript
dd.biz.util.previewImage({
    urls: [String],//图片地址列表
    current: String,//当前显示的图片链接
    onSuccess : function(result) {
        /**/
    },
    onFail : function() {}
})
```
##### 参数说明

参数 | 参数类型 |说明
----- | ----- | -----
urls | Array[String] | 图片地址列表
current | String | 当前显示的图片链接

#### 扫码

```javascript
dd.biz.util.qrcode({
    onSuccess : function(result) {
        /*
        {
            text: String
        }
        */
    },
    onFail : function(err) {}
})
```
##### 返回说明
参数 | 说明
----- | ------
text | 扫码的内容


#### 日期选择器

日期

注意：format只支持android系统规范，即2015-03-31格式为yyyy-MM-dd

```javascript
dd.biz.util.datepicker({
    format: 'yyyy-MM-dd',
    value: '2015-04-17', //默认显示日期
    onSuccess : function(result) {
        /*{
            value: "2015-02-10"
        }
        */
    },
    onFail : function() {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
format | String | format只支持android系统规范，即2015-03-31格式为yyyy-MM-dd
value | String | 默认显示日期

#####　返回说明
参数 | 说明
----- | ------
value | 返回选择的日期

时间

```javascript
dd.biz.util.timepicker({
    format: 'HH:mm',
    value: '14:00', //默认显示时间  0.0.3
    onSuccess : function(result) {
        /*{
            value: "10:00"
        }
        */
    },
    onFail : function() {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
format | String | 时间格式
value | String | 默认显示时间

#####　返回说明
参数 | 说明
----- | ------
value | 返回选择的时间

日期+时间

0.0.4

```javascript
dd.biz.util.datetimepicker({
    format: 'yyyy-MM-dd HH:mm',
    value: '2015-04-17 08:00', //默认显示
    onSuccess : function(result) {
        /*{
            value: "2015-06-10 09:50"
        }
        */
    },
    onFail : function() {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
format |String | 日期和时间的格式
value | String | 默认显示的日期和时间

#####　返回说明
参数 | 说明
----- | ------
value | 返回选择的日期和时间

### 下拉控件

0.0.5

```javascript
dd.biz.util.chosen({
    source:[{
        key: '选项1', //显示文本
        value: 123 //值，
    },{
        key: '选项2',
        value: 234
    }],
    onSuccess : function(result) {
        /*
        {
            key: '选项2',
            value: 234
        }
        */
    },
    onFail : function() {}
})
```
##### 参数说明

参数 | 参数类型 |  说明
----- | ----- | -----
source | Array[String] | 下拉控件的内容
key | String | 显示文本
value | String | 文本对应的值

#####　返回说明
参数 | 说明
----- | ------
key | 返回选择的文本
value | 返回选择的值

<!--## 聊天

dd.biz

### 发送消息

```javascript
dd.biz.chat.sendMessage({
    users : ['100', '101'],//用户列表，工号
    corpId: '', //企业id
    type: 1,//消息类型 1：image  2：link  [暂定和发钉保持一致]
    attachment: {
        images: [''],
    },
    text: String,
    onSuccess : function() {},
    onFail : function() {}
})
```

### 选择会话(群)

类似分享到钉钉的选取页面，只能选一个回话 0.0.3

```javascript
dd.biz.chat.chooseConversation({
    onSuccess : function(data) {
    /*
    {
        id: '123' //会话id
    }
    */
    },
    onFail : function() {}
})
```
#####　返回说明
参数 | 说明
----- | ------
id | 会话id

###获取会话信息

0.0.6

```javascript
dd.biz.chat.getConversationInfo ({
  cid: String, //会话ID
  onSuccess: function(data) {
  /* data结构
	  "cid": "1111", //会话ID
	  "title": '123', //会话标题
	  "avatarIcons": ["http://", ...], //会话的头像数组
	  "memberCount": 11, //群聊成员总数
	  }
  */
  },
  onFail : function(err) {
  }
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
cid | String | 会话ID

##### 返回说明
参数 | 说明
----- | -----
cid | 会话ID
title | 会话标题
avatarIcons | 会话的头像数组
memberCount | 群聊成员总数
-->

## Ding

dd.biz

### 发钉

#### 图片类型

0.0.3

```javascript
dd.biz.ding.post({
    users : ['100', '101'],//用户列表，工号
    corpId: '', //企业id
    type: 1, //钉类型 1：image  2：link
    attachment: {
        images: [''],
    }, //附件信息
    text: '', //消息
    onSuccess : function() {},
    onFail : function() {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
users | Array[String] | 用户列表，工号
corpId | String | 企业id
type | Number |Number为整数。钉类型 1：image，2：link
attachment | Object |附件信息
text | String |  消息

#### Link类型

0.0.3

```javascript
dd.biz.ding.post({
    users : ['100', '101'],//用户列表，工号
    corpId: '', //企业id
    type: 2, //钉类型 1：image  2：link
    attachment: {
        title: '',
        url: '',
        image: '',
        text: ''
    }
    text: '', //消息
    onSuccess : function() {},
    onFail : function() {}
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
users | Array[String] | 用户列表，工号
corpId | String | 企业id
type | Number |Number为整数。钉类型 1：image  2：link
attachment | Object | 附件信息
text | String |  消息

## 电话

dd.biz

### 打电话

0.0.3

```javascript
dd.biz.telephone.call({
    users: ['101', '102'], //用户列表，工号
    corpId: '', //企业id
    onSuccess : function() {},
    onFail : function() {}
})
```
##### 参数说明

参数 | 参数类型 |说明
----- | ----- | -----
users | Array[String] | 用户列表，工号
corpId | String | 企业id


## UI控件

dd.ui

### 输入框

0.0.2

```javascript
dd.ui.input.plain({
    placeholder: '说点什么吧', //占位符
    text: '', //默认填充文本
    onSuccess: function(data) {
        /*{
            text: String
        }*/
    },
    onFail: function() {

    }
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
placeholder | String | 占位符
text | String | 默认填充文本

##### 返回说明
参数 | 说明
----- | ------
text | 返回文本

### 设置顶部进度条颜色

0.0.5

```javascript
dd.ui.progressBar.setColors({
    colors: [], //array[number] 进度条变化颜色，最多支持4个颜色
    onSuccess: function(data) {
        /*
            true:成功  false:失败
        */
    },
    onFail: function() {
    }
})
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
colors | array[number] | 进度条变化颜色，最多支持4个颜色



### 启用下拉刷新

0.0.5

```javascript
dd.ui.pullToRefresh.enable({
    onSuccess: function(data) {
        //alert('下拉刷新')
    },
    onFail: function() {
    }
})
```

### 收起下拉loading

0.0.5

```javascript
dd.ui.pullToRefresh.stop()
```

### 禁用下拉刷新

0.0.5

```javascript
dd.ui.pullToRefresh.disable()
```

### 启用bounce

启用iOS webview弹性效果 0.0.5

```javascript
dd.ui.webViewBounce.enable()
```

### 禁用bounce

禁用iOS webview弹性效果 0.0.5

```javascript
dd.ui.webViewBounce.disable()
```

##扫码

dd.biz

0.0.6

```javascript
dd.biz.util.scan({
    type: String,//type为qrCode或者barCode
    onSuccess: function(data) {
      /* data结构
        { 'text': String}
      */
    },
   onFail : function(err) {
   }
})
```

##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
type | String | type为qrCode或者barCode

#####　返回说明

参数 | 说明
------|------
text | 扫码内容

## 企业通讯录

dd.biz

###选人
0.0.6

```javascript
dd.biz.contact.choose({
  multiple: Boolean, //是否多选： true多选 false单选； 默认true
  users: [Number, Number, ...], //默认选中的用户列表，工号；成功回调中应包含该信息
  corpId: String, //企业id
  max: Number, //人数限制，当multiple为true才生效，可选范围1-1500
  onSuccess: function(data) {
  /* data结构
    [{
      "name": "张三", //姓名
      "avatar": "http://g.alicdn.com/avatar/zhangsan.png" //头像图片url，可能为空
      "emplId": '0573', //工号
     },
     ...
    ]
  */
  },
  onFail : function(err) {}
});
```

##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
multiple | Boolean | 是否多选： true多选，false单选； 默认true
users | Array[Number] | 默认选中的用户列表，工号；成功回调中应包含该信息
corpId | String | 企业id
max | Number | 人数限制，当multiple为true才生效，可选范围1-1500

<!--
startWithDepartmentId: Number, //-1表示从自己所在部门开始, 0表示从企业最上层开始，(其他数字表示从该部门开始:暂时不支持)
startWithDepartmentId | Number | -1表示从自己所在部门开始, 0表示从企业最上层开始，(其他数字表示从该部门开始:暂时不支持)
-->

#####　返回说明

参数 | 说明
------|------
name | 姓名
avatar | 头像图片url，可能为空
emplId | 工号

###选人，选部门

0.0.6

```javascript
dd.biz.contact.complexChoose({
  startWithDepartmentId: Number, //-1表示从自己所在部门开始, 0表示从企业最上层开始，(其他数字表示从该部门开始:暂时不支持)
  selectedUsers: [Number, Number, ...], //预选用户
  corpId: String, //企业id
  onSuccess: function(data) {
  /* data结构
    {
      "users": [
      {
        "name": "张三", //姓名
        "avatar": "htp://g.alicdn.com/avatar/zhangsan.png" //头像图片url，可能为空
        "emplId": "0573", //工号:
      },
      ...
      ],
      "department": [
      {
        "id": 2,
        "name": "来往事业部",
      },
      ...
      ]
    }
  */
  },
  onFail : function(err) {}
});
```

##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
startWithDepartmentId | Number | -1表示从自己所在部门开始, 0表示从企业最上层开始，(其他数字表示从该部门开始:暂时不支持)
selectedUsers | Array[Number] | 预选用户
corpId | String | 企业id

<!--隐藏selectedDepartments
selectedDepartments | Array | 预选部门 id必选，name可选
selectedDepartments: [{name: '', id: ''}, ...],//预选部门 id必选，name可选
-->

#####　返回说明

参数 | 说明
------|------
users | 选取的用户
users.name | 姓名
users.avatar | 头像图片url，可能为空
users.emplId | 工号
department | 选取的部门
department.id | 部门id
department.name | 部门名称

### 选取

默认展示当前  业通讯录，开发中

```javascript
dd.biz.contact.choose({
    multiple: true, //是否多选： true多选 false单选； 默认true
    users: ['100','101'], //默认选中的用户列表，工号；成功回调中应包含该信息
    corpId: '', //企业id
    max: 50, //人数限制，当multiple为true才生效，可选范围1-1500
    onSuccess : function(result) {
        /*
        [{
            emplId: '0573', //工号
            name: '张三', //姓名
            nickNameCn: '三张', //花名
            mobilePhone: '****', //手机号 后续不再返回该字段
            emailAddr: '***', //邮箱  后期不再返回该字段
            pinyin: 'zhangsan', //姓名拼音
            avatar: 'htp://g.alicdn.com/avatar/zhangsan.png' //头像图片url，可能为空
        }]
        */
    },
    onFail : function(err) {}
});
```

##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
multiple | Boolean | 是否多选： true多选 false单选； 默认true
users | Array[String] | 默认选中的用户列表，工号；成功回调中应包含该信息
corpId | String |  企业id
max | Number| Number为正整数。人数限制，当multiple为true才生效，可选范围1-1500


#####　返回说明

参数 | 说明
------|------
emplID | 是否多选： true多选 false单选； 默认true
name | 姓名
nickNameCn | 花名
mobilePhone | 手机号，后续不再返回该字段
emailAddr | 邮箱，后期不再返回该字段
pinyin | 姓名拼音
avatar | 头像图片url，可能为空

### 创建企业群聊天

0.0.4

```javascript
dd.biz.contact.createGroup({
    corpId: '', //企业id，可选，配置该参数即在指定企业创建群聊天
    users: ['100','101'], //默认选中的用户工号列表，可选；使用此参数必须指定corpId
    onSuccess: function(result) {
        /*{
            id: 123   //企业群id
        }*/
    },
    onFail: function(err) {
    }
});
```

##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
corpId | String |  企业id，可选，配置该参数即在指定企业创建群聊天
users | Array[String] |  默认选中的用户列表，可选；使用此参数必须指定corpId

#####　返回说明

参数 | 说明
------|------
id | 企业群id

<!--### 添加好友

```javascript
dd.biz.contact.addFriend({
    onSuccess: function(result) {
        /*{
            id: 123   //企业群id
        }*/
    },
    onFail: function(err) {
    }
})
```
-->

<!--## 用户

### 获取当前用户信息

目前只开放emplId字段

```javascript
dd.biz.user.get({
    onSuccess : function(result) {
        /*
        {
            id: '112', //id
            emplId: '0573' //工号
        }
        */
    },
    onFail : function(err) {}
});
```
-->


## 导航栏

dd.biz

### 设置左侧导航按钮

```javascript
dd.biz.navigation.setLeft({
    show: false,//控制按钮显示， true 显示， false 隐藏， 默认true
    control: true,//是否控制点击事件，true 控制，false 不控制， 默认false
    showIcon: true,//是否显示icon，true 显示， false 不显示，默认true； 注：具体UI以客户端为准
    text: '',//控制显示文本，空字符串表示显示默认文本或icon
    onSuccess : function(result) {
        /*
        {}
        */
    },
    onFail : function(err) {}
});
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
show | Boolean | 控制按钮显示， true 显示， false 隐藏， 默认true
control | Boolean | 是否控制点击事件，true 控制，false 不控制， 默认false
showIcon | Boolean | 是否显示icon，true 显示，false 不显示，默认true； 注：具体UI以客户端为准
text | String | 控制显示文本，空字符串表示显示默认文本或icon


### 设置右侧导航按钮

```javascript
dd.biz.navigation.setRight({
    show: false,//控制按钮显示， true 显示， false 隐藏， 默认true
    control: true,//是否控制点击事件，true 控制，false 不控制， 默认false
    showIcon: true,//是否显示icon，true 显示， false 不显示，默认true； 注：具体UI以客户端为准
    text: '发送',//控制显示文本，空字符串表示显示默认文本或icon
    onSuccess : function(result) {
        /*
        {}
        */
    },
    onFail : function(err) {}
});
```

##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
show | Boolean | 控制按钮显示， true 显示， false 隐藏， 默认true
control | Boolean | 是否控制点击事件，true 控制，false 不控制， 默认false
showIcon | Boolean  | 是否显示icon，true 显示，false 不显示，默认true； 注：具体UI以客户端为准
text | String | 控制显示文本，空字符串表示显示默认文本或icon


可通过拼接url方式隐藏 showmenu=false

### 设置标题

```javascript
dd.biz.navigation.setTitle({
    title: '邮箱正文',//控制标题文本，空字符串表示显示默认文本
    onSuccess : function(result) {
        /*结构
        {
        }*/
    },
    onFail : function(err) {}
});
```
##### 参数说明

参数 | 参数类型 | 说明
----- | ----- | -----
title | String | 控制标题文本，空字符串表示显示默认文本

### 触发后退

```javascript
dd.biz.navigation.back({
    onSuccess : function(result) {
        /*result结构
        {}
        */
    },
    onFail : function(err) {}
})
```

## 附录

### JS-API权限签名算法

如果开发者想使用钉钉容器开放的jsapi接口，需要经过以下流程：

- 首先需要以[获取到的access_token](http://open.dingtalk.com/#建立连接)作为参数,获取jsapi_ticket。

- 然后在web页面加载到钉钉容器时，通过[jsapi权限验证配置接口](http://open.dingtalk.com/#页面引入js文件)验证可用的jsapi。这个接口使用的参数signature，在下文中有详细的说明。

#### 1.获取jsapi_ticket
jsapi_ticket,是开发者调用钉钉JS接口的临时授权码，其作用主要用于生成签名，这个签名在[jsapi权限验证配置接口](http://open.dingtalk.com/#页面引入js文件)中使用。

正常的情况下，jsapi_ticket的有效期为7200秒，通过access_token来获取。由于频繁刷新jsapi_ticket会导致api调用受限，影响自身业务，开发者必须在自己的服务全局缓存jsapi_ticket。
获取jsapi_ticket，具体可以[参考文档](http://open.dingtalk.com/#js接口api)
#### 2.签名生成算法
开发者在web页面使用钉钉容器提供的jsapi时，需要验证调用权限，并以参数signature标识合法性

签名生成的规则：

List keyArray = sort(noncestr,timestamp,jsapi_ticket,url);

String str = assemble(keyArray);

signature = sha1(str);

参与签名的字段包括在上文中获取的jsapi_ticket，noncestr（随机字符串）,timestamp（时间戳）,url（当前网页的URL，不包含#及其后面部分）。

- noncestr=Zn4zmLFKD0wzilzM
- jsapi_ticket=mS5k98fdkdgDKxkXGEs8LORVREiweeWETE40P37wkidkfksDSKDJFD5h9nbSlYy3-Sl-HhTdfl2fzFy1AOcKIDU8l
- timestamp=1414588745
- url=http://open.dingtalk.com

步骤1. sort()含义为对所有待签名参数按照字段名的ASCII 码从小到大排序（字典序）

步骤2. assemble()含义为根据步骤1中获的参数字段的顺序，使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串

步骤2. sha1()的含义为对在步骤2拼接好的字符串进行sha1加密。

### 图片缩放

通过JS-API获取的图片都支持缩放，比如图片：

`http://gtms02.alicdn.com/tps/i2/TB1SlYwGFXXXXXrXVXX9vKJ2XXX-2880-1560.jpg`

可通过文件名拼接的方法缩放为最大宽高为250x250的尺寸。

`http://gtms02.alicdn.com/tps/i2/TB1SlYwGFXXXXXrXVXX9vKJ2XXX-2880-1560.jpg_250x250.jpg`

拼接规则为:文件名+拼接规则；推荐使用以下拼接规则：



80x80 => _80x80.jpg

120x120 => _120x120.jpg

250x250 => _250x250.jpg

310x310 => _310x310.jpg

### 容器能力

[device.notification.alert](#alert)

[device.notification.confirm](#confirm)

[device.notification.prompt](#prompt)

[device.notification.vibrate](#vibrate)

[device.accelerometer.watchShake](#摇一摇)

[device.accelerometer.clearShake](#摇一摇)

[biz.util.open](#打开应用内页面)

[biz.util.openLink](#打开远程url)

[biz.util.share](#分享)

[biz.util.ut](#ut打点)

[biz.util.datepicker](#日期选择器) //android

[biz.util.timepicker](#日期选择器) //android

[biz.contact.choose](#选取)

<!--
[biz.user.get](#get)
-->

[biz.navigation.setLeft](#设置左侧导航按钮)

[biz.navigation.setRight](#设置右侧导航按钮)

[biz.navigation.setTitle](#设置标题)

[biz.navigation.back](#触发后退)


// v0.0.1

[device.notification.toast](#toast)

[device.notification.showPreloader](#showPreloader)

[device.notification.hidePreloader](#hidePreloader)

[device.geolocation.get](#get)

[biz.util.uploadImage](#上传图片)

[biz.util.previewImage](#图片浏览器)

//v0.0.2

[ui.input.plain](#输入框)

[device.notification.actionSheet](#actionSheet)

[biz.util.qrcode](#扫码)

[device.connection.getNetworkType](#获取当前网络类型)

[runtime.info](#info)

//v0.0.3

[biz.ding.post](#发钉)

[biz.telephone.call](#打电话)

[biz.chat.chooseConversation](#选择会话_群)

//v0.0.4

[biz.contact.createGroup](#创建企业群聊天)

[biz.util.datetimepicker](#日期选择器)

//v0.0.5

[biz.util.chosen](#下拉控件)

[device.base.getUUID](#获取设备唯一识别码)

[device.base.getInterface](#获取热点接入信息)

[device.launcher.checkInstalledApps](#检测应用是否安装)

[device.launcher.launchApp](#启动第三方应用)

[ui.progressBar.setRefreshing](#setRefreshing)

[ui.progressBar.setColors](#设置顶部进度条颜色)

[runtime.permission.requestAuthCode](#请求授权码)

[runtime.permission.requestJsApis](#请求jsapi)

[ui.pullToRefresh.enable](#启用下拉刷新)

[ui.pullToRefresh.stop](#收起下拉loading)

[ui.pullToRefresh.disable](#禁用下拉刷新)

[ui.webViewBounce.enable](#启用bounce)

[ui.webViewBounce.disable](#启用bounce)

## Demo

#### [Demo地址：](http://h5.m.laiwang.com/home/ding.html) `http://h5.m.laiwang.com/home/ding.html`
