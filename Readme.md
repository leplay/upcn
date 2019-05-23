# UP 中国区使用方法

[UP](https://github.com/apex/up) 是一个快速创建、部署 serverless 的服务。使用文档参考 [https://up.docs.apex.sh/](https://up.docs.apex.sh/)。

因为中国大陆 AWS 服务与其它地区的 API 有些区别，所以 UP 暂时不能在中国区使用。为了解决这个问题，在 UP v1.2.2 代码基础上做了些改动。目前达到**可用**状态。

## 改动部分
1. 增加了 cn-north-1 / cn-north-west-1 区域，分别代表 AWS 北京和 AWS 宁夏区域；（文件：./platform/aws/regions/regions.go）
2. 注释了 environment variables 相关代码，中国区暂不支持；（文件： ./platform/lambda/lambda.go）
3. 为 API Gateway 和 Lambda 增加了 .cn 的 endpoint；参考 [AWS 区域和终端节点](http://docs.amazonaws.cn/general/latest/gr/rande.html)。（文件： ./platform/lambda/lambda.go）
4. Binary 名称改为 upcn。（文件： install.sh 和 .goreleaser.yml）

## 安装
```
$ curl -sf https://raw.githubusercontent.com/leplay/upcn/master/install.sh | sh
```

## 使用方法
1. 重要！如果要在中国区使用 API Gateway 并且要使用公开 url 访问，需要去 AWS 支持中心创建案例申请开通；
2. 创建 up.json 要指定区域，并且将 cors 设置为 null，不要设置 environment 属性。示例：
```
{
  "name": "test",
  "profile": "up-cn",
  "regions": ["cn-north-1"],
  "cors": null
}
```
3. 在脚本目录里执行：
```
export AWS_REGION=cn-north-1
```
4. upcn deploy 之后，需要手动去 [AWS Lambda 后台](https://console.amazonaws.cn/lambda/home?region=cn-north-1) 添加一下 API Gateway 触发器。
