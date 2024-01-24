# OpenScreenAd


## 简介

开屏广告为用户在进入App时展示的全屏广告。开屏广告是单独Activity.

在广告SDK初始化成功后，可以使用开屏广告，需要在设置监听的之后调用show()方法。

## OpenScreenAd的使用

*  创建开屏广告

创建广告对象及请求参数

示例:

Java:
```java
String unitid = "a839bc20-6592-11ed-b410-4365750446c9";  // 申请到的unitid
// 创建 OpenScreenAd 实例，传入当前页面的context，广告unitid
OpenScreenAd openScreenAd = new OpenScreenAd(context, unitid);
openScreenAd.setTimeout(3000L) // 设置超时时间，默认3500

```
Kotlin:
```kotlin
val unitid = "a839bc20-6592-11ed-b410-4365750446c9";  // 申请到的unitid
private var appOpenAd: OpenScreenAd? = null

// 创建 OpenScreenAd 实例，传入当前页面的context，广告unitid
val openScreenAd = OpenScreenAd(SplashActivity.this, OpenScreenAdUnitId)
openScreenAd.setTimeout(3000L) // 设置超时时间，默认3500
```

openScreenAd.setTimeout(3000L)  是设置开屏的超时时间，当超过这个时间，开屏广告自动触发onAdLoadFail回调。App可以在onAdLoadFail回调里决定是否需要结束引导页跳转到主页的相关操作。 

* 设置回调

```java
public interface AdListener {

    /**
     * 广告点击
     */
    void onAdClick();

    /**
     * 广告关闭
     */
    void onAdClose();

    /**
     * 广告加载成功
     */
    void onAdLoadSuc();

    /**
     * 广告加载失败
     *
     * @param code 失败码
     * @param msg  失败信息
     */
    void onAdLoadFail(int code, String msg);

}
```

示例

Java:
```java
openScreenAd.setAdListener(new OpenScreenAdListener() {
    @Override
    public void onAdExposure() {
    
    }
    
    @Override
    public void onRenderFail(int code, String msg) {
    
    }
    
    @Override
    public void onAdClick() {
    
    }
    
    @Override
    public void onAdClose() {
        startActivity(new Intent(SplashActivity.this,MainActivity.class));
        finish();
    }
    
    @Override
    public void onAdLoadSuc() {
           showAdIfReady();
    }
    
    @Override
    public void onAdLoadFail(int code, String msg) {
    
    }
});
```

Kotlin:
```kotlin
openScreenAd?.setAdListener(object : OpenScreenAdListener {    @Override
    override fun onAdExposure() {
    
    }
    
    override fun onRenderFail(code: Int, msg: String) {
    
    }
    
    override fun onAdClick() {
    
    }
    
    override fun onAdClose() {
        startActivity(Intent(this, MainActivity::class.java))
        finish()
    }
    
    override fun onAdLoadSuc() {
           showAdIfReady();
    }
    
    override fun onAdLoadFail(code: Int, msg: String) {
    
    }
});
```

* 请求广告

通过openScreenAd.loadAd()方法使用请求广告, 由于需要网络请求和本地资源读取，属于耗时操作，可以在开屏启动页加载，建议可以在开屏广告关闭后调用，进行预加载下一次的开屏广告。  


```java
openScreenAd.loadAd();

```

* 展示开屏广告

Java:
```java
//  展示闪屏广告
private void showAdIfReady(){
   if ( openScreenAd == null || !FlatAdSDK.isInit ) return;

   if (openScreenAd.isReady() ){
       openScreenAd.show();
   }else{
       openScreenAd.loadAd();
   }
}
```

Kotlin:
```kotlin
//  展示闪屏广告
fun showAdIfReady(){
   if ( openScreenAd == null || !FlatAdSDK.isInit ) return;

   if (openScreenAd?.isReady() ){
       openScreenAd?.show();
   }else{
       openScreenAd?.loadAd();
   }
}
```
* openScreenAd.isReady() 是判断当前广告是否准备好可以展示，当isReady返回ture时，说明已经加载好开屏广告，可以用于展示。  

* openScreenAd.show() 是打开开屏广告，show的过程不需要网络。  

 


* 参数说明

|参数|含义|
|:----|:----|
|unitId|广告位ID|
|timeOut|超时关闭时间|

