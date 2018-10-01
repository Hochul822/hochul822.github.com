- 광고
  - Banner
  - Interstital
  - Rewarded Video

#### 안드로이드 구글 애드몹


### 애드몹 설정

```gradle
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:26.1.0'
    implementation 'com.google.android.gms:play-services-ads:15.0.1'
}
```


```xml
<meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="AD-MOB ID" />
```

AD BANNER

```xml
<com.google.android.gms.ads.AdView
    xmlns:ads="http://schemas.android.com/apk/res-auto"
    android:id="@+id/adView"
    android:layout_width="match_parent"
    android:layout_height="200dp"
    ads:adSize="LARGE_BANNER"
    ads:adUnitId="ca-app-pub-3940256099942544/6300978111">
</com.google.android.gms.ads.AdView>
```

```java
adView = findViewById(R.id.adView);
adView.loadAd(new AdRequest.Builder().build());
```

AD Interstial

```java
button = findViewById(R.id.button);

interstitialAd = new InterstitialAd(this);
interstitialAd.setAdUnitId("ca-app-pub-3940256099942544/1033173712");
interstitialAd.loadAd(new AdRequest.Builder().build());

interstitialAd.setAdListener(new AdListener(){
    @Override
    public void onAdOpened() {
        super.onAdOpened();

    }
});

button.setOnClickListener(event -> {
    if (interstitialAd.isLoaded()) {
        interstitialAd.show();
    }
});
```
