# New InterstitialAd

### 新插屏广告实现
1. 创建 FlatInterstitialRequest 对象（非必须）
2. 加载插屏广告和注册FlatInterstitialAdLoaderListener回调
3. 注册广告行为事件回调
4. 展示广告

新插屏广告创建使用 **FlatInterstitialAd** 调用静态方法 **FlatInterstitialAd.loadAd()** 方法加载广告。
其中`loadAd()`方法的参数`adUnitId`为广告位id，`FlatInterstitialRequest`为额外的请求参数，默认情况下可不传，
`FlatInterstitialAdLoaderListener`为新插屏广告加载的监听。

### 创建`FlatInterstitialRequest`对象
```kotlin
val request = FlatInterstitialRequest()
```
### 加载插屏广告和注册`FlatInterstitialAdLoaderListener`回调
使用FlatInterstitialAd中的loadAd方法加载广告，并且注册`FlatInterstitialAdLoaderListener`回调。
```kotlin
FlatInterstitialAd.loadAd(
    "YOUR_AD_UNIT_ID",
    request,
    object : FlatInterstitialAdLoaderListener {
        override fun onAdLoaded(ad: FlatInterstitialAd) {
        }
        
        override fun onError(code: Int, message: String) {
        }
    }
)
```

#### `FlatInterstitialAdLoaderListener` 回调说明
| FlatInterstitialAdLoaderListener | 说明                                                                  | 
|----------------------------------|---------------------------------------------------------------------|
| onError                          | 此方法在广告加载失败时调用。它包含一个error类型的错误参数，指示发生了什么类型的故障。要了解更多信息，请参阅ErrorCode部分 |
| onAdLoaded                       | 此方法在广告素材成功加载时执行。                                                    |

### 注册广告行为事件回调
广告行为事件回调需要在显示广告之前注册。事件回调中的每个方法对应于广告生命周期中的一个事件。
```kotlin
flatInterstitialAd.setAdListener(object : FlatInterstitialAdInteractionListener {
    override fun onAdClicked() {
        
    }
    
    override fun onAdDismissed() {
        
    }
    
    override fun onAdShowed() {
        
    }
    
    override fun onResourceLoadSuc() {
        
    }
    
    override fun onResourceLoadFail(msg: String) {
    }
})
```
#### `FlatInterstitialAdInteractionListener` 回调说明
| FlatInterstitialAdInteractionListener | 说明           |
|---------------------------------------|--------------|
| onAdClicked                           | 广告被点击时调用。    |
| onAdDismissed                         | 广告被关闭时调用。    |
| onAdShowed                            | 广告被展示时调用。    |
| onResourceLoadSuc                     | 广告资源加载成功时调用。 |
| onResourceLoadFail                    | 广告资源加载失败时调用。 |

### 展示广告
当广告成功加载时，将返回FlatInterstitialAd对象的一个实例。调用FlatInterstitialAd的show()方法来呈现广告，它需要传入activity，并且必须在主线程中调用。

```kotlin
if (flatInterstitialAd?.isReady == true) {
    flatInterstitialAd.show(currentActivity)
}
```

### 关于预加载
在关闭广告时，可以在onAdDismissed回调中调用FlatInterstitialAd的loadAd方法重新加载广告。

<font color=red>注意：flatInterstitialAd实例展示完后无法再次展示，需要重新调用loadAd方法重新加载广告并且获取新的FlatInterstitialAd实例进行展示。</font>


<br>
<footer style="text-align: right;">更新时间：2024-01-09 16:00:00</footer>
