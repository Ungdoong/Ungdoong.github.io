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



## 빌드에러 

## : Execution failed for task ':app:generateDebugBuildConfig'.

___________

### 현상

Execution failed for task ':app:generateDebugBuildConfig'.

> java.io.IOException: Unable to delete directory C:\s02p22a107\foowipe\frontend2\android\app\build\generated\source\buildConfig\debug\com.

### 이유

 이전에 빌드 내용이 잔재해 중복 로드되어 나타남

### 해결 방법

1.  gradlew clean

   ```bash
   $ cd android
   $ ./gradlew clean
   ```

2. build 폴더 삭제

   android > app > build 폴더 삭제



## 포트가 이미 사용중일 경우

____________

### 해결 방법

 다른 포트로 실행

```bash
$ react-native run-android --port=8082
```





## Execution failed for task ':app:packageDebug'

___________

### 현상

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:packageDebug'.

> java.io.IOException: Unable to delete directory C:\s02p22a107\foowipe\frontend2\android\app\build\intermediates\incremental\packageDebug\tmp\debug.

### 이유

 여러가지 라이브러리를 가져다 쓰는 중에 라이센스 파일 충돌 혹은 라이브러리 중복 로드로 인해 발생

### 해결 방법

 Build.Gradle(:app)의 android 스크립트 내부에 아래의 코드 추가

```
packagingOptions {
    exclude 'META-INF/DEPENDENCIES.txt'
    exclude 'META-INF/LICENSE.txt'
    exclude 'META-INF/NOTICE.txt'
    exclude 'META-INF/NOTICE'
    exclude 'META-INF/LICENSE'
    exclude 'META-INF/DEPENDENCIES'
    exclude 'META-INF/notice.txt'
    exclude 'META-INF/license.txt'
    exclude 'META-INF/dependencies.txt'
    exclude 'META-INF/LGPL2.1'
}
```

