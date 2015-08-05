# FAQ

## Q:从哪里可以获得CorpID和CorpSecret?

A:在钉钉管理后台的微应用设置页面可以获取，详细方法请参见[《接入指南-获取CorpID和CorpSecret》](http://open.dingtalk.com/#获取corpid和corpsecret)。

## Q:调用管理通讯录接口返回430004，无效的HTTP HEADER Content-Type如何解决？

A:管理通讯录的部分接口采用了POST请求，请求体使用JSON格式，请在HTTP请求头中设置Content-Type:application/json。

## Q:开发遇到困难怎样反馈给你们?

A:我们目前在阿里云开发者论坛开放了一个[钉钉开放平台](http://bbs.aliyun.com/thread/276.html?spm=5176.7189909.0.0.bq46VP)模块，你可以按照以下格式发帖到这个模块，我们会定期搜集和解决。

请编辑如下信息，发到群里面，这样便于快速解决问题。

企业完整名称 | 例如 阿里巴巴
---------- | -------
访问接口的全路径 | 例如 https://oapi.dingtalk.com/gettoken
输入参数 | AccessToken及CorpId、CorpSecret不需要提供
发起请求的服务器的外网IP地址 |
发起请求的时间点 | 例如 2015-6-16 10:03:50
返回的错误结果 | 例如 系统繁忙

若你急需找到我们请联系业内同事朋友在钉钉上加入"钉钉开放平台官方技术答疑群"进行反馈。