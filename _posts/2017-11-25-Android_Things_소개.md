---
title: 'Android Things 소개'
date: 2017-11-25 12:00:00
categories:
- andtoid-things, iot, android
tags:
- android
- things
- iot
- android-things
thumbnail: https://developer.android.com/things/index.html
author : 안귀정
---

# Android Things 소개

## Android Things 란?

Android Things 란 무엇일까요? 

Android Things Android 플랫폼으로 사물인터넷(IoT) 까지 확장을 지원하는 것입니다. Android 는 이미 성공한 플랫폼으로 핸드폰 뿐만 아니라 TV, Wearable, Auto 등을 지원합니다.

<img src="http://postfiles8.naver.net/MjAxNzExMjNfMjU1/MDAxNTExNDAxNDYyODY3.fONywhTQOi414KjQwt0G8UKoWiADAGnmkY_rzw4itYgg.bhBp7gMFq227Wrrw88uEBGKSJD8BSCxmarnKYXJKvpog.PNG.akj61300/스크린샷_2017-11-23_오전_10.44.12.png?type=w773" />

안드로이드가 여러 플랫폼에 빠르게 확장될 수 있었던 이유는 안드로이드 플랫폼 자체가 오픈소스인 것과 무료로 사용할 수 있는 정책때문이라고 생각됩니다.

오픈소스의 특성에 따라 안드로이드는 여러 플랫폼의 확장을 개발자들이 자발적으로 개발하는 문화가 형성되어 있습니다. 라즈베리파이, 아두이노등 같은 MCU 의 안드로이드를 올리는 프로젝트 역시 없던 프로젝트는 아닙니다.

Android Things 가 다른 오픈소스들과 다른 점은, 구글이 주도하에 개발하고 있다는 것입니다. 즉 꾸준하고 품질높은 지원이 될 가능성이 높다는 것입니다.

Android Things 는 구글 홈페이지에서 다음과 같이 소개중입니다.

<img src="http://postfiles13.naver.net/MjAxNzExMjNfMTQy/MDAxNTExNDIxODY1NzI2.AMLu2j5KdxALjFE4DU5NvR0oNJepFS3s5S5_mJxJK5Qg.cSxClq8La0rolD1Ll2Pxt3EmqcBYWKniGVgKLl_XaD8g.PNG.akj61300/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2017-11-23_%EC%98%A4%ED%9B%84_4.23.35.png?type=w2" />

* The ease and power of Android
* Rapid prototypes to real products
* The power of Google at your fingertips

요약하면 안드로이드를 사용하니 유용하고 강력하면서 프로토타입 개발이 쉽고, 구글이 강력하게 지원하겠다 정도로 보입니다.

실제로 Android Things 를 스터디 후 필자가 느낀 점은 안드로이드 개발자 입장에서는 쉽다는 것입니다. IoT 개발이지만 거부감이 거의 없다는 것이죠.


근래에 가장 핫한 IT 트렌드 중 하나인 IoT 를 안드로이드로 할 수 있다니, 게다가 구글이 지원한다니 매우 흥미진진 하지만, 한가지 궁금증이 생깁니다. 대체 IoT 가 무엇일까요?

## IoT 는 뭘까?

Android Things 는 IoT 플랫폼이니까 공부하기 위해서는 먼저 IoT 가 무엇인지 고민해 볼 필요가 있습니다. 최근 핫한 트렌드이기 때문에 IoT 라는 용어 자체는 친숙하게 느껴지지만 정작 IoT 가 뭐냐고 묻는다면 사물인터넷? 이라는 추상적인 대답만 떠오릅니다.

추상적이긴 하지만 IoT 는 말 그대로 사물인터넷입니다. Internet Of Things 의 약자니까요. 이 추상적인 개념을 조금더 구체적으로 보기위해서 2017 년 출시된 IoT 제품들을 살펴보겠습니다.

---

#### 스마트 도어락

<img src="http://postfiles11.naver.net/MjAxNzExMjNfMjc3/MDAxNTExNDA0NzQ3NzI3.VQ9yYCEepznjTfmZjjiJeZ1dZRBt6oV0Y0BpzaRmzasg.eOACO0IIdSbOhuW71BZC3qUsjKdSPPP-CMkmdOEuNbcg.GIF.akj61300/DoorSense-Silver-Auto-Lock-closeup_3_1.gif?type=w773" />

스마트 도어락은 기존에 사용된 잠금장치에 대해 사물인터넷을 적용한 것입니다. 주된 기능은 다음과 같습니다.

* 원격에서 현재 잠금 상태를 확인하고 제어 가능.
* 친구, 가족 등록등을 사용해 키가 없이 잠금 및 잠금해제 가능. 키의 복제 잃어버릴 위험이 없음.
* 음성인식으로 다른 IoT 제품들과의 연계

#### 스마트 자전거 잠금장치 트랙커

<img src="http://postfiles4.naver.net/MjAxNzExMjNfMTAx/MDAxNTExNDA1MTk4Mzg1.vTb-yph7emeQuY_0VcMpqM7cjBapT_SfLnwC3KhH-IIg.EyhJjB5JHbPeTREAawCiy94nnKHlLaSorUdU_v1_-wEg.PNG.akj61300/스크린샷_2017-11-23_오전_11.46.20.png?type=w773" width="500px" />

스마트 자전거 잠금장치 트랙커는 자전거 잠금장치에 IoT 를 결합해 키없이 자전거를 잠금 또는 해제 할 수 있고, 더불어 GPS 기반으로 Geo Tagging 을 제공합니다.

#### 스마트 스피커

<img src="http://postfiles1.naver.net/MjAxNzExMjNfMTc3/MDAxNTExNDA0Mjk5MjA0.u2v1jjobA1OtjBXAJCNj6vXcAzImafRi9AzgHvLEQoUg.VHGMIlZf0PfHKdr8BK2N5QgrV4uqqrgZA3M2_W5-GjAg.PNG.akj61300/스크린샷_2017-11-23_오전_11.31.12.png?type=w773" width="500px" />

스마트 스피커는 스피커에 IoT 를 결합한 상품이지만 핵심은 스마트 홈을 제어하는데에 목적이 있습니다. 애플의 시리 삼성의 빅스비처럼 사용자의 음성명령을 인식하고 다른 IoT 장비를 제어하는 것입니다. 

---

2017 IoT 제품들을 보면 이미 익숙하게 느껴지는 것도 있을것이고 저런것 까지 스마트해야하는가 라는 물음표가 생기는 제품도 있을 것입니다. 

이런것까지 스마트해야하는가? 라는 물음표가 어쩌면 IoT 의 핵심을 관통하는 것일수도 있습니다. IoT 에서 마지막 단어인 Things 는 모든 사물을 말하는 것이기 때문입니다. 

이미 2017년에 IoT 제품으로 소개된 스마트 스피커, 도어락 등의 익숙한 제품이 아니더라도 우리가 일상에서 만나는 모든 사물이 IoT 의 대상입니다. 침대, 컵, 칫솔 일상생활에서 만날 수 있는 모든것이 대상인 것이죠.

IoT 에 모든 사물이라는 대상적 측면을 제외하고 중요한 측면은 Internet 입니다. 사물들이 스마트해질 수 있게 하기 위한 핵심중 하나가 사물끼리 데이터 통신을 가능하게 하여 정보를 교류 하는 것이기 때문입니다. 

즉 IoT 는 일상생활에서 접하는 모든 사물에 인터넷 기술을 접목하여, 센서를 통해 데이터를 수집하고 그 데이터를 학습해 지능형 서비스를 제공하는 등의 인프라 서비스를 말한다고 볼 수 있습니다.

## Android Things 를 왜 하는가?

IoT 를 공부하기 위해서 Android Things 를 꼭 공부해야하는 것일까요? 

결론부터 말하면 아닙니다.

IoT 를 공부해보기 위한 다른 많은 플랫폼들이 있습니다. 라즈베리 파이를 이용할 경우 RASPBIAN, Pidora, UBUNTU MATE 등 다양한 다른 방법들이 있습니다. 

하지만 Android Things 를 선택한다면 얻을수 있는 몇가지 장점들이 있습니다.

#### Android 와 유사한 개발방법

IOT 임베디드 장치를 Android 개발하듯이 개발 가능합니다. 다음은 Android 프레임워크 레이어에서 Android Things 의 위치를 보여줍니다.

<img src="http://postfiles5.naver.net/MjAxNzExMjNfMTIw/MDAxNTExNDEyNzMwNDM5.Z_guDYHcC0veC2U2YVguGiQ7LjwL-OjequurFwVMYdog.Zl-bEUnA9FbHOySIgG8hi6fnPz2z4zBSfa59smW9bRYg.PNG.akj61300/스크린샷_2017-11-23_오후_1.51.47.png?type=w773" />

Android Things 의 레이어는 Google Service 처럼 App 이 사용하는 라이브러리와 비슷하게 동작합니다. 즉 일반 안드로이드 앱을 개발하는데 임베디드 라이브러리 의존성이 추가된 형태와 비슷합니다.

플랫폼 자체가 안드로이드이므로 Camera, Bluetooth, 오디오 재생 등 다양한 기능들을 기존 안드로이드처럼 개발 가능합니다. 

때문에 진입장벽이 낮습니다. 이것은 매우 큰 메리트중 하나라고 생각합니다.

#### Google Service 사용이 쉽다

안드로이드와 같은 개발방법을 사용하기 때문에 강력한 구글 서비스를 그대로 사용 가능합니다. 쉽고 강력한 백엔드 기술인 Firebase, Google Cloud Service 등 모든 구글 서비스를 사용 가능합니다. 또 자체적으로 데이터를 학습하기 위해 TensorFlow 를 활용할 수 있습니다. 

물론 Android Things 가 아니더라도 Firebase, Google Cloud, TensorFlow 같은 서비스를 사용할 수 없는 것은 아닙니다. 

하지만 기존에 다른 플랫폼에서 다양한 구글 서비스를 적용하는 것이 마냥 쉬운 작업은 아닙니다.

Android Things 는 이런 구글 서비스들을 모두 쉽게 적용가능하다는 장점이 있습니다.

#### 구글이 책임지는 플랫폼 업데이트와 OTA

IoT 서비스 중 중요한 이슈중 하나는 보안 이슈입니다. 사물에 인터넷 서비스가 들어간다는 이야기를 다시 생각해보면 만약 보안이 취약해 해킹을 당했을때의 그 피해 역시 막대할 것으로 쉽게 생각 해볼 수 있습니다.

<img src="http://postfiles8.naver.net/MjAxNzExMjNfOTYg/MDAxNTExNDE0MTYyMjM3.3m68fXrLipN4sFjEmY8fXb2LyhfF1mV7IpHz96K2hKwg.ZiSyctpPGYCOTUegqLv50IooHD0obZAummUK_cmWrfcg.PNG.akj61300/스크린샷_2017-11-23_오후_2.15.43.png?type=w773" />

Android Things 를 사용하는 경우, 플랫폼적 측면에 업데이트는 구글이 책임지게 됩니다. 안드로이드의 보안 업데이트나 월간 보안 패치가 있다면 해당 기기에 자동으로 배포되게 됩니다. 따라서 개발자는 플랫폼적 업데이트는 신경쓰지 않고 앱 개발에 초점을 맞출 수 있습니다.

## Android Things 를 시작하기 위해 준비해야할 것들

Android Things 를 해보기 위해서는 다음과 같은것들이 필요합니다.

#### 하드웨어

Android Things 가 공식적으로 지원하는 하드웨어는 다음과 같습니다.

* [NXP Pico i.MX7D](http://www.nxp.com/AndroidThingsGS)
* [NXP Pico i.MX6UL](http://www.nxp.com/AndroidThingsGS)
* [NXP Argon i.MX6UL](http://www.nxp.com/AndroidThingsGS)
* [NXP SprIoT i.MX6UL](http://wireless.murata.com/eng/products/wireless-connectivity-platforms/iot-system-on-module/spriot-6ul.html)
* [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/)
* [Intel® Edison](https://software.intel.com/en-us/iot/android-things)
* [Intel® Joule](https://software.intel.com/en-us/iot/android-things)

#### 소프트웨어

안드로이드 개발과 비슷하기 때문에 안드로이드 개발환경이 필요합니다. 구체적으로 보면 다음과 같습니다.

* Windows or OSX or Linux 운영체제
* Android Studio 및 Android SDK 툴

## Next Step

다음 컨텐츠에서는 라즈베리파이 3 에 Android Things 로 이미지로 부팅후 LED 1개를 제어하는 것까지 소개합니다.


