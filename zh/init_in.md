# Flat Ads SDK 添加依赖


## 添加依赖
```gradle
dependencies {
		// 需要联系产品经理获取定制化版本号，邮件交付
    implementation 'com.flatads.sdk:flatads:xxxx' 
}

allprojects {
    repositories {
        maven {url "http://maven.flat-ads.com:8084/repository/maven-public/"}
				maven { url "https://jitpack.io" }
    }
}

buildscript {
    repositories {
				maven {url "http://maven.flat-ads.com:8084/repository/maven-public/"}
        maven { url "https://jitpack.io" }
    }
}
```
