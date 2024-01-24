# Interactive

## 简介

互动广告

* 创建互动广告入口View

```xml
<com.flatads.sdk.ui.view.InteractiveView
  android:id="@+id/interactive_view"
  android:layout_width="50dp"
  android:layout_height="50dp"
  />
```

```java
String adUnitId = "xxxxxxxxx";

interactiveView = findViewById(R.id.interactive_view);
interactiveView.setAdUnitId(adUnitId);
interactiveView.setCacheTime(1000 * 10);
```

* 设置广告监听

```java
interactiveView.setAdListener(new InteractiveAdListener() {
    @Override
    public void onRenderSuccess() {
    }

    @Override
    public void onRenderFail(int code, String msg) {
    }

    @Override
    public void onAdClick() {
    }

    @Override
    public void onAdClose() {
    }

    @Override
    public void onAdLoadSuc() {
    }

    @Override
    public void onAdLoadFail(int code, String msg) {
    }
});
```

* 广告请求

广告请求成功后, 会自动渲染入口图标显示在屏幕上

```java
interactiveView.loadAd();
```


setCacheTime为设置缓存时间，默认为24小时。  

如果开发者想指定互动广告icon图片，提供了方法设置：
```
interactiveView.setIconView(Drawable);
interactiveView.setIconView(Bitmap);
interactiveView.setIconView(url);
```  

注意：Activity销毁时需要调用onDestroy方法   
```
interactiveView.onDestroy();
```  

注意：
1.互动广告需要尽早的调用，如可在进入app时进行互动广告加载，在需要展示互动广告时，把view添加到布局上面去。
2.广告回调onAdExposure时，则webview已经加载完成。
