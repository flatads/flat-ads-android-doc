# Flat Ads SDK 添加依赖


|版本:v1.4.18.2


## 集成方式:Gradle集成

### 步骤一: 添加仓库

在**project**级别的**build.gradle**文件中添加**Maven**的引用，url 'http://maven-pub.flat-ads.com/repository/maven-public/'

示例:
```gradle

allprojects {
    repositories {
        maven {url "http://maven-pub.flat-ads.com/repository/maven-public/"}
				maven { url "https://jitpack.io" }
    }
}

buildscript {
    repositories {
				maven {url "http://maven-pub.flat-ads.com/repository/maven-public/"}
        maven { url "https://jitpack.io" }
    }
}
```

### 步骤二: 添加依赖
在主module的build.gradle文件添加SDK依赖

```gradle
dependencies {
    implementation 'com.flatads.sdk:flatads:1.4.18.2-Flat-GP-20231214.040004-2'
}

```


## AndroidManifest配置

添加权限
```xml
<!--必要权限-->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
 <uses-permission android:name="com.google.android.gms.permission.AD_ID" />
```


## 运行环境配置

本SDK可运行于Android4.4 (API Level 19) 及以上版本。<font color="#008000">\<uses-sdk android:minSdkVersion="19" android:targetSdkVersion="31" /\></font>


### 代码混淆

如果您需要使用proguard混淆代码，需确保不要混淆SDK的代码

**注意**: SDK代码被混淆后会导致广告无法展现或者其它异常

```
-keep class com.flatads.sdk.** {*;} 
```


## 初始化SDK

Flat Ads SDK可以在主线程或者子线程初始化。

初始化SDK时候需要传入申请时候获取的appid和token。


### SdkConfig 作用


```
isDebug: 是否测试环境, 正式发布请设置为false
adDomain: 请求广告的域名,默认情况请不要调用
logDomain: 日志上报的域名,默认情况请不要调用
```


---
Java:

```java
import com.flatads.sdk.FlatAdSDK;
import com.flatads.sdk.config.SdkConfig;
import com.flatads.sdk.callback.InitListener;

public class MyApplication extends Application {

    @Override
    protected void onCreate() {
        
        String appId = "xxxxxxxx"; //申请时的appid
        String token = "xxxxxxxxxxxxxxxx"; //申请时的token

        SdkConfig config = new SdkConfig();
        
        FlatAdSDK.initialize(getApplication(), appId, token, new InitListener(){
            @Override
            public void onSuccess() {
                // 可以请求开屏广告
            }
            @Override
            public void onFailure(int code, String msg) {
                // 初始化失败
            }
        },config);
    }
    
}
```

---

Kotlin:

```kotlin
import com.flatads.sdk.FlatAdSDK;
import com.flatads.sdk.config.SdkConfig;
import com.flatads.sdk.callback.InitListener;

class MyApplication : Application {

    override fun onCreate() {
        
        val appId = "xxxxxxxx" //申请时的appid
        val token = "xxxxxxxxxxxxxxxx" //申请时的token

        val config:SdkConfig  =  SdkConfig()
        
        FlatAdSDK.initialize(getApplication(), appId, token, object : InitListener {
            override fun onSuccess() {
                // 可以请求开屏广告
            }
            
            override fun onFailure(code: Int, msg: String) {
                // 初始化失败
            }
        },config)
    }
    
}
```

onFailure回调状态码

|value|说明|
|:----|:----|
| 2001  | 初始化失败 |



初始化状态接口


```java
public interface InitListener {

    /**
     * Flat Ads SDK 初始化成功
     */
    void onSuccess();


    /**
     * Flat Ads SDK 初始化失败
     *
     * @param code 失败码
     * @param msg  失败信息
     */
    void onFailure(int code, String msg);
}

```
