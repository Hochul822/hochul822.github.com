2018.11.8

- Define the FileProvider in your AndroidManifest file
- Create an XML file that contains all paths that the FileProvider will share with other applications
- Bundle a valid URI in the Intent and activate it

파일 공유를 할 때, 안드로이드 7.0부터는 Uri.parse()를 쓰면 안된다.

대신 FileProvider.getUriForFile()를 써야한다.

##### 이전 방식
```java
Intent intent = new Intent(Intent.ACTION_SEND);
intent.setType("image/*");
intent.setPackage(packageName); // com.kakao.talk
intent.putExtra(Intent.EXTRA_STREAM, Uri.parse("file:///" + outputName));
```

##### 새로운 방식
Uri.parse대신 FileProvider.getUriForFile을 써줘야한다.

```java
Intent intent = new Intent(Intent.ACTION_SEND);
intent.setType("image/*");
intent.setPackage(packageName); // com.kakao.talk
Uri uri = FileProvider.getUriForFile(this, getPackageName(), new File(outputName));
intent.putExtra(Intent.EXTRA_STREAM, uri);
```

AndroidManifest.xml도 수정해줘야한다. <application> 안에 추가하도록 하자.

```xml
<provider
    android:name="android.support.v4.content.FileProvider"
    android:authorities="${applicationId}"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/filepaths" />
</provider>
```

xml 폴더를 만들고 filepaths.xml를 만들어주자.

```xml
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <external-path name="storage/emulated" path="."/>
</paths>
```

이제 제대로 공유가 된다!

참고 기사 ( https://infinum.co/the-capsized-eight/share-files-using-fileprovider)

----
