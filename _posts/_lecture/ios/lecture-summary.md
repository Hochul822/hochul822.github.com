
- 새 기능을 추가하고 싶을때?
 - 스택 오버플로우를 검색해보자.
 - https://forums.developer.apple.com 에서 검색을 해봐라
 - 공식 api나 doc는 보기에 좋진 않다.


- PodFile 설정 바꾸거나, 라이브러리 추가할 때 사용
  - 위치 : ios/PodFile
  - ios 버전 설정 :
    - platform: ios, '10.0'
  - 라이브러리 추가 :
    - pod = 'Firebase/Core'


- 에러 코드로 에러난 것 찾기 (NS OS Status Error Domain)
  - OSStatus.com에 가서 에러난 코드를 입력해보면 된다.

- IOS 10에서는 어떤 기능을 사용할 때마다 사용자 확인 메시지를 설정해줘야한다.
  - 디버그 메시지
    This app has crashed because it attempted to access privacy-sensitive data without a usage description. The app’s Info.plist must contain an NSCameraUsageDescription key with a string value explaining to the user how the app uses this data.

  - 해결법 : Info.plist에서 Privacy 설정을 해주면 된다.

  - 출처: http://kka7.tistory.com/40


  - 테이블뷰, 콜렉션뷰 쓸때 주의할점
  : 꼭 delegate랑 datasource를 연결했는지 확인하자.


- NSData : Byte 데이터를 처리할 때 쓰인다.
