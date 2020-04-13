# Vue Native 에러별 해결 방법



## Manifest merger failed 병합 에러

_____________

### 현상

> Manifest merger failed : Attribute application@appComponentFactory value=(android.support.v4.app.CoreComponentFactory) from [com.android.support:support-compat:28.0.0] AndroidManifest.xml:22:18-91        is also present at [androidx.core:core:1.0.1] AndroidManifest.xml:22:18-86 value=(androidx.core.app.CoreComponentFactory).
>    Suggestion: add 'tools:replace="android:appComponentFactory"' to <application> element at AndroidManifest.xml:7:5-117 to override.

### 이유

기존의 com.android.support.* 또는 android.support.*와 Android Architecture Component에서 사용하던 android.arch.*의 패키지명이 안드로이드 28.0.0부터 새로운 **androidx.\*** 패키지 명으로 교체되어 발생

관련링크 - https://android-developers.googleblog.com/2018/05/hello-world-androidx.html

### 해결 방법

1.  `gradle.propertis` 파일에 아래 내용을 추가하고 sync now

   ```properties
   android.useAndroidX=true
   android.enable.Jetifier=true
   ```

2. 안드로이드 스튜디오에서 refactor → migrate to androidx 수행