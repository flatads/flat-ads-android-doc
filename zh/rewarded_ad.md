# Rewarded Ad 

## 简介

激励广告是一种在应用内向用户提供奖励的广告形式。激励广告的主要目的是鼓励用户进行积分兑换，从而获得更多的应用内虚拟货币。

* 创建广告

创建广告对象及请求参数

```java
RewardedAd rewardedAd = new RewardedAd(this,adUnitId);
```

* 设置广告回调

```java
public interface RewardedAdListener extends AdListener {

    /**
     * 广告曝光
     */
    void onAdExposure();  

    /**
     * 用户获取奖励
     */
    void onUserEarnedReward();

    /**
     * 广告播放失败
     */
    void onAdFailedToShow();
}
```

| 名称           | 类型    | 说明      |
|:-------------|:------|:--------|
| onAdLoadSuc  | void  | 广告加载成功  |
| onAdLoadFail | void  | 广告加载失败  |
| onAdExposure | void  | 广告曝光    |
| onAdClick    | void  | 广告点击    |
| onAdDestroy  | void  | 广告销毁    |
| onUserEarnedReward | void | 用户获取奖励 |
| onAdFailedToShow | void | 广告播放失败 |

示例

Java
```java
rewardedAd.setAdListener(new RewardedAdListener() {
    @Override
    public void onAdClose() {
    }

    @Override
    public void onUserEarnedReward() {
    }

    @Override
    public void onAdFailedToShow() {
    }

    @Override
    public void onAdExposure() {
    }

    @Override
    public void onAdClick() {
    }

    @Override
    public void onAdLoadSuc() {
    }

    @Override
    public void onAdLoadFail(int code, String msg) {
    }
});
```

Kotlin
```kotlin
rewardedAd?.setAdListener(object : RewardedAdListener {
    override fun onAdClose() {
    }

    override fun onUserEarnedReward() {
    }

    override fun onAdFailedToShow() {
    }

    override fun onAdExposure() {
    }

    override fun onAdClick() {
    }

    override fun onAdLoadSuc() {
    }

    override fun onAdLoadFail(code: Int, msg: String?) {
    }
})
```

* 广告请求

通过rewardedAd.loadAd()方法使用请求广告, 由于需要网络请求和本地资源读取，属于耗时操作

```java
rewardedAd.load();
```

* 广告展示

通过rewardedAd.show()方法展示激励广告

```java
rewardedAd.show();
```


  




