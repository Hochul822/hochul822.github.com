2018.10.31

##### 안드로이드

apply from: "$flutterRoot/packages/flutter_tools/gradle/flutter.gradle"

때문에 에러가 생기면

flutter.gradle에서 jcenter()의 위치를 가장 아래로 내려보자!


###### iOS 크래시리틱스, 패브릭 추가
crashlytics가 objective-c 라이브러리다보니

podfile에서 그냥 추가하면은 에러가 생긴다.

use_framework를 지워주고,

podfile에 아래 코드를 추가해주자.
```
pre_install do |installer|
        # workaround for https://github.com/CocoaPods/CocoaPods/issues/3289
        Pod::Installer::Xcode::TargetValidator.send(:define_method, :verify_no_static_f    ramework_transitive_dependencies) {}
end
```

----

2018.11.12
위젯의 width나 height를 infinity로 하려면 Expanded안에 넣어야한다.
