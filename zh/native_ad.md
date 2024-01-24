# Native 广告

## 简介

Native显示样式由用户自定义，需要向布局中添加NiveAdLayout和MediaViewat，但需要调用NativeAdLayout作为父类，其中媒体用MediaView加载，NativeAdLayout为FrameLayout。

<font color="red">注意：不支持开发者在view添加按钮及对广告拦截处理。</font>

## Native 广告的使用

* 创建广告对象以及请求参数

在布局添加NativeAdLayout，在布局中添加MediaView，MediaView需要设置宽高比例。

```xml

<com.flatads.sdk.ui.view.NativeAdLayout android:id="@+id/container"
    android:layout_width="match_parent" android:layout_height="wrap_content">

    <androidx.constraintlayout.widget.ConstraintLayout android:layout_width="match_parent"
        android:layout_height="match_parent">
        ···
        <com.flatads.sdk.ui.view.MediaView android:id="@+id/media"
            android:layout_width="match_parent" android:layout_height="0dp"
            app:layout_constraintDimensionRatio="w,9:16"
            app:layout_constraintTop_toTopOf="parent" />
        ···
    </androidx.constraintlayout.widget.ConstraintLayout>

</com.flatads.sdk.ui.NativeAdLayout>
```


* 从布局中获取广告View和创建广告对象

```java
NativeAdLayout nativeAdView = findViewById(R.id.container);
String adUnitId = "xxxxxxxxx";
NativeAd nativeAd = new NativeAd(this,adUnitId);
```

* 设置广告监听

```Java
public interface NativeAdListener {

    /**
     * 广告加载成功
     */
    void onAdLoadSuc(Ad ad);

    /**
     * 广告加载失败
     *
     * @param errorCode 失败码
     * @param msg  失败信息
     */
    void onAdLoadFail(int errorCode, String msg);

    /**
     * 广告曝光
     */
    void onAdExposure();

    /**
     * 广告点击
     */
    void onAdClick();

    /**
     * 广告销毁
     */
    void onAdDestroy();

    /**
     * 渲染失败
     * @param code 失败码
     * @param msg 失败信息
     */
    void onRenderFail(int code, String msg);
}

```

| 名称           | 类型    | 说明      |
|:-------------|:------|:--------|
| onAdLoadSuc  | void  | 广告加载成功  |
| onAdLoadFail | void  | 广告加载失败  |
| onAdExposure | void  | 广告曝光    |
| onAdClick    | void  | 广告点击    |
| onAdDestroy  | void  | 广告销毁    |



示例:

Java
```java
 nativeAd.setAdListener(new NativeAdListener() {
    @Override
    public void onAdLoadSuc(Ad ad) {
    }

    @Override
    public void onAdLoadFail(int errorCode, String msg) {
    }

    @Override
    public void onAdExposure() {
    }

    @Override
    public void onAdClick() {
    }

    @Override
    public void onAdDestroy() {
    }

    @Override
    public void onRenderFail(int code, String msg) {
    }
    
 });

```

Kotlin
```kotlin
nativeAd?.setAdListener(object : NativeAdListener {
    override fun onAdLoadSuc(ad: Ad) {
    }

    override fun onAdLoadFail(errorCode: Int, msg: String?) {
    }

    override fun onAdExposure() {
    }

    override fun onAdClick() {
    }

    override fun onAdDestroy() {
    }

    override fun onRenderFail(code: Int, msg: String?) {
    }
});
```



* 广告请求

广告请求属于异步方法, 会通过网络请求广告, 当广告请求成功后会在onAdLoadSuc回调, 当请求失败时会在onAdLoadFail回调并返回错误码和错误信息.

```java
nativeAd.loadAd();
```


* 广告渲染

当请求广告成功后, 根据返回的广告信息进行广告渲染, 把广告的信息赋值到自定义的NativeAdLayout的布局中, 并且使用registerViewForInteraction方法将广告对象(NativeAd)和广告View(NativeAdLayout)绑定

```java
private void inflateAd(Ad ad) {

    adView = (NativeAdLayout) getLayoutInflater().inflate(R.layout.native_layout, null);

    TextView tvTitle = adView.findViewById(R.id.flat_ad_tv_title);
    TextView tvDesc = adView.findViewById(R.id.flat_ad_tv_desc);
    TextView tvAdBtn = adView.findViewById(R.id.flat_ad_button);
    View view = adView.findViewById(R.id.flat_ad_container);
    ImageView icon = adView.findViewById(R.id.flat_ad_iv_icon);
    MediaView mediaView = adView.findViewById(R.id.flat_ad_media_big);

    tvTitle.setText(ad.getTitle());
    tvDesc.setText(ad.getDesc());
    tvAdBtn.setText(ad.getAdBtn());

    // Create a list of clickable views
    List<View> clickableViews = new ArrayList<>();
    clickableViews.add(tvAdBtn);
    clickableViews.add(view);

    nativeAd.registerViewForInteraction(adView, mediaView, icon, clickableViews);
}



```

* 当广告在瀑布流列表中划出时, 需要解绑广告View并进行View对象的释放

```java
nativeAd?.destroyView()
adView?.destroy()
adView = null
```

* 当广告重新回到屏幕中时, 在Adapter的onBindViewHolder()中重新绑定即可

```java
@Override
public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
    // 从缓存中获取广告对象 nativeAd
    // 从ViewHolder获取复用的AdView
    ...
    // 绑定广告View和广告对象
    nativeAd.registerViewForInteraction(adView, mediaView, icon, clickableViews);
    ...
}
```

* 单独跟页面使用时, 广告跟页面的生命周期绑定

```java
@Override
protected void onPause() {
    super.onPause();
    if (adView!=null){
        adView.pause();
    }
}

@Override
protected void onResume() {
    super.onResume();
    if (adView!=null){
        adView.resume();
    }
}

@Override
protected void onStop() {
    super.onStop();
    if (adView!=null){
        adView.stop();
    }
}

@Override
protected void onDestroy() {
    super.onDestroy();
    if (nativeAd != null){
        nativeAd?.destroyView()
        nativeAd?.destroy()
    }
    if (adView!=null){
        adView.destroy();
        adView = null
    }
}
```


布局元素获取必须在成功的时候绑定，处理完布局后需要调用nativeAd.registerViewForInteraction()方法展示广告，否则无反应。

由于native广告存在video类型的广告，需要在Activity生命周期中对应添加广告的生命周期回调，否则播放器可能会异常。

