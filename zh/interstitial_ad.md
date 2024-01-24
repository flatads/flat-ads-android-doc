# InterstitialAd

## 简介

插屏广告, 开发者不用自行对广告样式进行编辑和渲染，可直接调用相关接口进行广告展示。

注意：不支持开发者在view添加按钮及对广告拦截处理。


* 创建广告对象以及请求参数

```java
InterstitialAd interstitialAd = new InterstitialAd(this,adUnitId);
```

* 设置广告监听

```java
interstitialAd.setAdListener(new InterstitialAdListener() {
    @Override
    public void onAdLoadSuc() {
    }

    @Override
    public void onAdClose() {
    }

    @Override
    public void onAdLoadFail(int code, String msg) {
    }

    @Override
    public void onAdExposure() {
    }

    @Override
    public void onRenderFail(int code, String msg) {
    }

    @Override
    public void onAdClick() {
    }
});
```

* 加载广告

```java
interstitialAd.loadAd();
```


* 展示广告


  需要在请求广告完成时再调用show方法展示广告。
```java
interstitialAd.show();
```

