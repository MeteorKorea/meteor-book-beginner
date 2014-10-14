# Cordova Integration

## 설치

### iOS

개발한 앱을 iOS Simulator나 iOS 단말기에서 실행하려면, Xcode가 필요하다.  
Xcode를 열고 라이선스 조항에 동의하여 설치를 완료한다.

### Android

특별한 설치 요구사항은 없다.  
Android 버전을 처음 개발하는 경우, 미티어가 필요한 부분을 bundle에 다운로드하고 설치한다.
(호스트 플랫폼에 따라 다르지만 대략 300MB~350MB 정도 차지한다.)

JDK가 설치되지 않았다면, 이를 설치해야 한다. (설치하라는 메시지가 나올 것이다.)

만약 Linux 배포판 64-bit 버전이면, 별도의 32-bit 라이브러리들을 설치해야 할 것이다.

Ubuntu 14.04 버전이라면, 다음과 같다:
```
  sudo apt-get install lib32z1 lib32stdc++6
```

시스템에 이미 Android Development Kit이 설치되어 있다면, 미티어가 다시 설치하지 않도록 모든 command에 
다음 환경변수를 설정하도록 한다:

```
  env USE_GLOBAL_ADK=t meteor run android      # will run android and adb
```

iOS나 Android 단말기에서 Meteor 앱을 실행하거나 디버깅하는 방법을 알고 싶으면,
단말기 설정 정보를 보기 바란다.


## Command Line에서 하기

### 플랫폼 추가/삭제

모든 미티어 프로젝트는 복수의 "platform"에서 구동시키고자 한다. 
플랫폼은 미티어 프로젝트에 `meteor add-platform` 명령어로 추가할 수 있다.

`meteor add-platform ios` 명령은 프로젝트에 iOS 플랫폼을 추가한다.  
`meteor add-platform android` 명령은 프로젝트에 Android 플랫폼을 추가한다.  
`meteor remove-platform ios android` 명령은 프로젝트에서 iOS, Android 플랫폼을 삭제한다.  
`meteor list-platforms` 명령은 앱의 플랫폼 목록을 출력한다.


