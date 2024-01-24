# 业务异常码

说明：正常情况status返回1

| 状态码（status） | 说明 | 描述（msg) | 
| --- | --- | --- |
| 40003 | 签名验证失败  |  not validate   |
| 40101 | 广告位不存在    |  Not bidding:tagid not actived   |
| 40102 | adx 流控  |  request next time please   |
| 40103 | 没有匹配的广告返回 |  no ads from all dsps   |
| 40201 | 参数错误      |  empty appid or sign   |


# 注意事项
1、接入时需对app开启存储权限后才可以正常下载广告配置的apk，否则部分手机将无下载反应。  

2、混淆时，需添加 -keep class com.flatads.sdk.response.* {*;} ，否则将无数据返回。  

3、native布局添加元素时，需要在loadAd方法之前添加，否则无法正常显示。  

4、插屏广告需要注册回调监听且在onAdLoadSuc中调用show方法展示广告，否则将无法正常显示广告。