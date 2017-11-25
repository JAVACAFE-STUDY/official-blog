---
title: 'Android Things - 부팅부터 LED 제어까지'
date: 2017-11-26 09:00:00
categories:
- andtoid-things, iot, android, led, booting
tags:
- android
- things
- iot
- android-things
- led
thumbnail: https://developer.android.com/things/index.html
author : 안귀정
---

# Android Things - 부팅부터 LED 제어까지

이번 컨텐츠는 Android Things 를 이용해 라즈베리파이3 를 부팅해보고, LED 한개를 제어해보는 것이 목적입니다.

## 준비물

* Rassberry PI 3
* Bread Board
* Micro SDCARD 
* SDCARD Adapter(PC 에 Micro SD 카드를 인식시키기 위한 어댑터)
* 소켓점프케이블 M/F 2개
* 저항 220 옴
* LED 1개

<img src="http://postfiles4.naver.net/MjAxNzExMjVfMjc1/MDAxNTExNTY1MzcyMjg0.Q3vmIPsZGLomnmi9ISsqGm83HA1rjKYXsUfzJw8TE1sg.bTjZGkQ2o3yMBCHG9zMypCYw1n3HwSVI2Wq_yuffQ08g.PNG.akj61300/readt.png?type=w773" width="600px" />

## 라즈베리파이3 에 Android Things 이미지로 부팅해보기

라즈베리파이 3 에 Android Things 로 부팅하기 위해서는 먼저 Android Things OS 로 부팅할 수 있는 SD 카드를 만들어야 합니다. 마치 PC 부팅을 위해서는 윈도우즈를 설치하는 것과 비슷합니다.

Android Things 시스템 이미지를 다운로드 받기 위하여 Android Things 콘솔을 방문합니다.

[https://partner.android.com/things/console](https://partner.android.com/things/console)


Android Things 는 기존에는 시스템 이미지를 직접 다운로드 하는 링크가 있었지만, 현재에는 웹에서 OTA 를 지원하기 위하여 웹 콘솔 형태로 변경되었습니다.

Android Things Console 에 접속하면 다음 화면처럼 나올 것입니다. 프로덕트를 추가합니다.

<img src="http://postfiles16.naver.net/MjAxNzExMjVfMzAg/MDAxNTExNTY1OTk2MTE0.ic3iuTdY8w1gHK0Lh0DDiHgJRJm2bcGi5BcAZbltgFIg.OpuvLK67I3k-2ROPpV2h2VrV--N9_KOzTLl3r0T4dykg.PNG.akj61300/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2017-11-25_%EC%98%A4%EC%A0%84_8_24_51.png?type=w773" width="400px" />

다음 팝업에서는 생성할 제품에 이름, SOM 타입 등을 설정하는 화면입니다. 제품이름에는 FirstThings, SOM Type 에는 라즈베리파이3 을 선택합니다.

<img src="http://postfiles10.naver.net/MjAxNzExMjVfMjc5/MDAxNTExNTY2MTgzMTI5.t11LScBpOPKY3NhuTgt8MaDJzqu9F2X_ZAluy5s9Pcsg.5DHirxqyI28BFqh8hp8U4EC4gl9mx1EYSn8BZR_-zhUg.PNG.akj61300/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2017-11-25_%EC%98%A4%EC%A0%84_8.28.01.png?type=w773" width="400px" />

Create 버튼으로 제품을 생성하면 제품 설정화면이 나오게 됩니다. 상단 탭에서 Factory Images 탭을 선택합니다.

<img src="http://postfiles9.naver.net/MjAxNzExMjVfMjI3/MDAxNTExNTY2Mzk4Njc3.8wLnoDuXvmjkA6AmPjmDtK4PXOTWldg0UFudQ1dxbmcg.aSy0lr-BKndQxe-Em5YFLxjUuJQWdPoOiPaDcD1oflcg.PNG.akj61300/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2017-11-25_%EC%98%A4%EC%A0%84_8_31_53.png?type=w773" />

Factory Images 하단에 Android Things 버전을 고르고 CREATE BUILD CONFIGURATION 을 클릭합니다.

<img src="http://postfiles10.naver.net/MjAxNzExMjVfNjYg/MDAxNTExNTY2NjYyNDE4.SN4ioKSpiT0ks4XEs9qBSDrNSe89Cq8Veul9OvvMO8Qg.bAvCdwsUflzCVjqhx5e-ajvFZgLMp-jFPdvdiOpQx8Yg.PNG.akj61300/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2017-11-25_%EC%98%A4%EC%A0%84_8_34_45.png?type=w773" />

조금 시간이 걸리고 작업이 완료되면 Build configuration list 항목이 새로 생긴 것을 확인할 수 있습니다. 리스트 항목 우측에 Download Build 버튼을 클릭합니다.

<img src="http://postfiles2.naver.net/MjAxNzExMjVfMjQx/MDAxNTExNTY3MDUyMzg5.oATJ8lrknmMsAeXSg33paML0DmqyI2Go4Ec2nqDHhlAg.FAW5qAF7SSSeUBDqspcG54FHcwq0RyKA2cimj2-wN5kg.PNG.akj61300/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2017-11-25_%EC%98%A4%EC%A0%84_8_43_03.png?type=w773" />

다운로드가 끝나면 이제 다운로드 받은 이미지로 SDCARD 를 구울 차례입니다. 이미지를 굽기 위해서 [Etcher](https://etcher.io/) 라는 프로그램을 추천합니다. 윈도우, OSX, 리눅스 모든 플랫폼에서 사용가능하고 사용방법이 매우 간단합니다. Etcher 을 다운로드 하기 위해 Etcher 홈페이지를 방문합니다.

[https://etcher.io/](https://etcher.io/)

Etcher 홈페이지에 방문하여 운영체제에 맞는 버전을 다운로드 합니다.

<img src="http://postfiles7.naver.net/MjAxNzExMjVfMjgx/MDAxNTExNTY3NDQ1MTQ3.F3-78_2Pe9kLTPywamOMy89685yjuGQ0ptjPnAEOjgIg.K0N13iP5eGmDGrka6f19BdCHIWNR5XD3ZL4aA6WimVUg.PNG.akj61300/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2017-11-25_%EC%98%A4%EC%A0%84_8_47_55.png?type=w773" /> 

다운로드 및 설치가 완료되면 Etcher 를 실행하세요. Etcher 사용법은 간단합니다. 구울 이미지를 선택하고 어디에 구울지 선택하면 됩니다. 먼저 Select Image 버튼을 누르고 조금전에 다운로드 한 Android Things 이미지 압축파일을 선택하세요.

<img src="http://postfiles5.naver.net/MjAxNzExMjVfMzMg/MDAxNTExNTY4MTA4NjM1._VnoyNxjS2SzZoPJrXaRwbEfc_C9GoC56Ox0WdOAsPUg.te_t73Nc2weMXnhlUk2zNdRZCx2t4sHaeX8SV73JUPEg.GIF.akj61300/11%EC%9B%94-25-2017_09-01-18.gif?type=w773" width="320px" />

PC 에 micro SD 카드를 연결하세요. 필자의 경우 맥북에 micro sd 카드를 바로 인식시킬수 없어 SD 카드 어댑터를 사용했습니다. 어떤 형태로든 Micro SD 카드를 인식할수 있으면 됩니다. 

Etcher 은 외장 디스크가 하나인 경우 그것을 자동으로 선택합니다. 만약 다른 드라이브에 이미지를 구워야 한다면 change 버튼을 눌러 드라이브를 변경합니다.

<img src="http://postfiles3.naver.net/MjAxNzExMjVfMTcw/MDAxNTExNTY5Mjg2ODY4.a5u6hXcB3wrerSVdSxnz51ZxrfeVGjRPm8TQ9PAiJKUg.0NekZO19b9eZuXEfp_h5SFe9maD5SvxIN5T85CiSo0kg.PNG.akj61300/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2017-11-25_%EC%98%A4%EC%A0%84_9_20_02.png?type=w773" />

이미지와 드라이브 선택이 완료되었다면 이제 Flash 버튼으로 이미지를 굽기만 하면됩니다.

<img src="http://postfiles5.naver.net/MjAxNzExMjVfMTg3/MDAxNTExNTY5MzYxNjE1.sA__8niS2XlbRBIk0mMuvjhhIUrbG_phqPMrwvFDfFQg.7vnOqCd0IX9LcMS98FXne45NRQqcjUt3S8QBGIbbI8sg.PNG.akj61300/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2017-11-25_%EC%98%A4%EC%A0%84_9_20_02.png?type=w773" />

이미지를 굽는 작업은 컴퓨터 성능에 따라 시간이 상당히 걸릴수 있습니다. 차라도 한잔 마시면서 기다리면 됩니다.

이미지를 Micro SD 카드에 굽는 과정이 완료되었다면 이제 라즈베리파이에 Micro SD 카드를 마운트하면 됩니다.

라즈베리파이를 잘살펴보면 Micro SD 카드를 마운트 할것처럼 생긴 곳이 있습니다. 보통의 경우에는 USB 포트 반대편 쪽에 뒤집은 위치에 있습니다. 보드의 적혀있는 영어를 잘살펴보면 MICRO SD CARD 라고 적혀있습니다.

<img src="http://postfiles10.naver.net/MjAxNzExMjVfMjMz/MDAxNTExNTcwOTA0ODM1.FdRRtO17DvraQW5IRCOYwWUXqrcIUSqrW-l3Y6aLmtIg.rsNxOe8hrsevET-FpabJkbQjvQssFBXhfMxqXQ7zaXwg.PNG.akj61300/DSC_6025.png?type=w773" width="500px"/>

Micro SD 카드를 라즈베리파이 3 에 방향을 잘 맞춰서 마운트해주세요.

<img src="http://postfiles5.naver.net/MjAxNzExMjVfMjMz/MDAxNTExNTcxNDExMzg2.XIgByeegxnYtjQ8pDNR8zJajqDo7efiJTW0KCa_pZ_Yg.__2hHxsmBuaL1Vf2pkbKIv70-tk-Weyr0AC1jFCK03Eg.JPEG.akj61300/DSC_6026.jpg?type=w773" width="500px" />

이제 라즈베리파이3 를 부팅하면 됩니다. 화면을 보기위하여 HDMI 로 모니터에 연결하고 부팅해보겠습니다.

모니터에 다음과 같은 화면이 나온다면 정상적으로 모든것이 진행된 것입니다.

<img src="http://postfiles14.naver.net/MjAxNzExMjVfMTY0/MDAxNTExNTcxOTMxMjA0.kyOXjPWF7EfR01tIwWtFydq7wrXUOBNZ94h2ErO4U1Eg.YZPrJw_YA1DnQU7Fwre3jl3NwjWD0qfb0RyzwAb_Rhgg.JPEG.akj61300/DSC_6029.jpg?type=w773" />

## 개발환경을 설정하자

Android Things 는 다른 안드로이드 개발을 할때 처럼 ADB 라는 툴을 이용해 디바이스와 연결하고 프로그램을 설치합니다. 

그렇기 때문에 Android Things 역시 ADB 연결을 해야만 개발을 시작할 수 있습니다. 이제 어떻게 ADB 를 라즈베리파이 3와 연결할 수 있는지 살펴보겠습니다.

> 여기서 ADB 의 설치방법을 다루지는 않겠습니다. 구글에 안드로이드 개발환경 설정을 검색하여 ADB 및 안드로이드 개발환경 설정을 해주세요.

먼저 라즈베리파이에 다른 안드로이드 기기처럼 USB 로 ADB 연결을 가능하게 하는 5핀 케이블 포트를 찾아보세요. 딱 한개 보이긴 하지만 그것은 전원으로 사용합니다.

딱히 ADB 로 사용할 포트가 없으니 USB 말고 다른 ADB 연결 방법을 사용하는 것이 편리합니다. 다른 연결방법이라는 것은 바로 ADB 의 TCP 모드입니다.

Android Things 는 ADBD(ADB 데몬) 가 기본적으로 TCP 모드로 동작되게 설계되어 있습니다.(보통의 경우에는 TCP 모드가 부팅하자마자 동작하지는 않습니다) 

즉 Android Things 디바이스에 이더넷 케이블을 꼽고 ADB TCP 연결을 하면 된다는 이야기입니다.

라즈베리파이에 랜선을 연결하고 다시 부팅을 시도합니다. 화면에 Eth0 IP 주소가 나올 것입니다.

<img src="http://postfiles3.naver.net/MjAxNzExMjVfNjcg/MDAxNTExNTc1MDc4MTQ1.oKR_ysddiwLZSctXN9F2NvzLLzzgnPRo4t_mPZyr-9Ug.PKAqSQqQdBewjmhyzs0P1wo0nyLNlcfGSOkT_gpP908g.JPEG.akj61300/DSC_6031.jpg?type=w773" />

이제 라즈베리파이3 와 개발 PC 를 같은 랜(공유기)에 접속하고 터미널에 다음 명령을 입력하세요.

```bash
adb connect $라즈베리파이_IP_주소
```

필자의 경우는 라즈베리파이3 의 IP 주소가 192.168.0.23 이므로 다음과 같이 입력합니다.

```bash
adb connect 192.168.0.23
```

<img src="http://postfiles4.naver.net/MjAxNzExMjVfNiAg/MDAxNTExNTc1Mzk5Mzcx.i_ExxrQm-37kbVAKEEGetQh1lfdOOagcm2fApZRa_fQg.UMh3keHlyzn8Wsq9fGtmrHa3fiMna_5ikwicIQXGVV4g.PNG.akj61300/adb_connect.png?type=w773" width="500px" />

정상적으로 연결이 되었는지 확인하기 위해 다음 명령어를 터미널에 입력합니다.

```bash
adb devices
```

<img src="http://postfiles2.naver.net/MjAxNzExMjVfMTU4/MDAxNTExNTc1NDAxMjE1.HFt-o19QbYPapwHicnOAl-H9OTnt80KvPenMqc3hpTUg.4ozZ31giSnDhiREwP2sc5ruLRgW4TVox_pD2WRgHmBUg.PNG.akj61300/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2017-11-25_%EC%98%A4%EC%A0%84_11.02.22.png?type=w773" width="500px" />

이제 ADB 가 연결되었기 때문에 평소 안드로이드 개발하듯이 라즈베리파이3 에 프로그램을 업로드 할수 있게 되었습니다.

## LED 를 켜기위한 회로를 만들자

LED 를 라즈베리파이로 제어하기 위한 회로를 만들어보겠습니다. 먼저 LED 의 구조를 보면 다음과 같습니다.

<img src="http://postfiles5.naver.net/MjAxNzExMjVfMjMz/MDAxNTExNTc2Njg1MjE5.zpYCK4wEc8QE-wej04SjSooT102a7n7TSELbSUyBVIQg.amNN_UhVPRjUNSuCI5-H2VV5UARDaPw9ckLEH6QIidAg.PNG.akj61300/led-cathode-anode.png?type=w773" />

꽤 복잡한 구조이지만 사실 모두 알 필요는 없습니다. 오히려 LED 구조중 눈여겨 봐야할 부분은 +, - 부분입니다. LED 를 자세히 보시면 한쪽이 길고 한쪽은 짧은것을 알수가 있습니다. 긴 쪽에 + 극을 연결해야하며, 짧은 쪽에 - 극을 연결해야 합니다.

<img src="http://postfiles9.naver.net/MjAxNzExMjZfMjcz/MDAxNTExNjM0Nzg2NDEz.P9mBdynwjTazZqPDfssvsOozAiPjK9NJxW_T8n4p85wg.lemKLykvqH0q3MEfDCupbUve7x5h0lMWWUJna3oCCw0g.PNG.akj61300/led_1.png?type=w773" width="150px" />

다음은 BreadBoard(빵판) 을 보겠습니다. 빵판을 보시면 구역이 나눠져 있는것을 알 수 있습니다. 구역이 나눠진 이유는 구역별로 흐르는 전류가 공유되기 때문입니다.

예를 들어 다음과 같이 전기가 연결되어 있는 경우를 생각해보겠습니다.

<img src="http://postfiles4.naver.net/MjAxNzExMjZfMTYx/MDAxNTExNjM2Mjk1ODk0.hU8-3ZexnXn58J4g8hSTRn2Wv36L-lkmJBVuDqnYmvAg.BAGDB7HV0iJhsoA7j90gebVnAmePAtz33mkJrUaDxxsg.PNG.akj61300/bread02.png?type=w773" width="400px" />

건전지의 양(+)극이 빵판 좌측 첫번째 칸에 연결되어 있습니다. 그렇다면 그림의 빨간 색으로 색칠된 영역에는 전부 + 극 전류가 흐르게 됩니다. 음(-)극이 연결 된 빵판 우측 6번째 칸에는 초록색 영역 전부 음극이 연결된것입니다.

또 가장 측면에 있는 영역은 보통 +, - 극 전원 전용으로 쓰입니다. 이 영역은 세로로 전류가 공유됩니다. 예를 들어 건전지가 다음과 같이 연결되어 있는 경우를 보겠습니다.

<img src="http://postfiles6.naver.net/MjAxNzExMjZfMjg2/MDAxNTExNjM2ODMyMDQ0.7KiEL05rq8pKPM2k_I25PXjE0MvByIXgf1jH9FwB4ccg.JUsOXd8vlXWGL4ekDO38deYdAryD_oZeHIVOPb-Ae9sg.PNG.akj61300/bread03.png?type=w773" width="400px" />

건전지의 + 극이 좌측 첫번째 칸에 연결되어 있습니다. 이런 경우에 빨간색으로 색칠되어진 모든 칸이 전부 + 극이 됩니다. 건전지의 - 극 마찬가지로 초록색 영역전부가 - 극이 됩니다. 즉 세로로 전류가 공유됩니다.

다음은 라즈베리파이의 PIN 정보를 보겠습니다. 라즈베리파이의 핀정보는 [https://pinout.xyz/](https://pinout.xyz/) 에 자세하게 나와있습니다.

<img src="http://postfiles15.naver.net/MjAxNzExMjZfMjQ3/MDAxNTExNjM3MjU3MzQw.wgDFYrrtk4Fal4CjKP72P4nLsQ0rpuVJTlYoATNHqCQg.9X8EOXxdYX9Du_WUAJXG2cT8ebmeg7iOkboNOL0RoQUg.PNG.akj61300/pinout01.png?type=w773" width="400px" />

여기서 사용할 핀은 Ground 와 GPIO 핀인 BCM6 핀입니다.

<img src="http://postfiles4.naver.net/MjAxNzExMjZfMjc5/MDAxNTExNjM3NTk0NDg1.cW0RAHr7ekPtfyEcWz-Yaf8cZub9NCJQAOgDkW_BBcgg.YmL9v7XNnlG2f5mN2OO7zLUolqjBn7BRSrvH7EtSW9Qg.JPEG.akj61300/pinout02.jpg?type=w773" width="400px" />

6번 Ground 핀과 31번 BCM6 핀에 M/F 점퍼케이블을 연결하고 빵판에 다음과 같이 연결합니다.

<img src="http://postfiles8.naver.net/MjAxNzExMjZfMjQ1/MDAxNTExNjM3OTgzMzU1.sweMC6xZXGJimVACyInxw6A0jHwO3ZFWLfy9pmiAMLEg.aq_sqFlOHOMPc_ELqUVvUdBAKSeTw19SXyGDkTDr4hwg.JPEG.akj61300/Circuit01.jpg?type=w773" width="400px" />

라즈베리파이의 핀을 정확하게 연결하고 빵판에 전류가 흐를수 있게 연결되는 것이 중요합니다. 또 LED 의 +, - 극도 잘 확인하고 연결하는게 좋습니다. 실제로 필자가 스터디할때 다른 부분보다 회로를 만드는 부분에서 삽질이 많았습니다.

회로를 연결한 실제 사진입니다.

<img src="http://postfiles1.naver.net/MjAxNzExMjZfMTAz/MDAxNTExNjQyMjMwNzQ3.kG2a1lhbeIT-gPeOjOn0XyTbwxgfsgZcm_qir6k74PIg.r4kT3TWUFMJMGisTqYrnQxLvb25Hq02YkMRzm9BkVCQg.JPEG.akj61300/DSC_6034.jpg?type=w773" width="600px" />

## GROUND(GND), GPIO 란?

앞서 회로를 연결하며 라즈베리파이의 핀아웃으로 GPIO 와 GROUND 를 연결한다고 하였는데 이것들이 무엇인지 잠시 살펴보도록 하겠습니다.

|용어|설명|
|:--|:--|
|GROUND(GND)| 흔히 회로에서 음(-)극을 나타냅니다. 보다 정확하게 표현한다면 전기회로에서 기준전위를 나타냅니다. 예를 들어 5V 는 GND 보다 5V 만큼 높다는 의미입니다. 음(-) 극을 GROUND 라고 부르는 것은, 기준 전위가 지구의 땅의 전위를 기준으로 잡기 때문입니다. |
| GPIO | General Purpose Input/Ouput 의 줄임말입니다. 말 그대로 핀의 출력을 조절할수가 있습니다. 예를 들어 핀의 출력을 0V 또는 3.3V 로 조절할 수 있습니다. 또 입력모드로 사용하는 경우 핀에 0V 가 걸리는 경우와 3.3V 가 걸리는 경우를 구분하여 판단할 수 있습니다. |

전기회로에서 전기는 전위차에 의해 흐르게 됩니다. GND 는 기준전위를 나타내기 때문에 음극(-) 으로 사용하는 것입니다. 

GPIO 는 핀의 출력을 프로그래밍으로 0V 또는 3.3V 로  조절이 가능합니다. 따라서 이번 LED 제어 프로그램에서 GPIO 의 출력을 조절하면서 LED 를 제어할 예정입니다. 

## Android Studio 로 Android Things 프로젝트를 생성해보자

이제 Android Studio 로 LED 제어 프로그램을 만들어보겠습니다. 안드로이드 스튜디오에서 프로젝트를 새로 생성합니다. 생성하는 화면에서 어플리케이션 이름을 FirstAndroidThings, Company Domain 은 유니크한 것으로 만들고, Kotlin 지원을 체크해주세요.

<img src="http://postfiles4.naver.net/MjAxNzExMjZfNjgg/MDAxNTExNjQyNDYzNjAx.0p677JFPXojA_EdyDJpiMItMVrGe6G6r27VDBiacMLAg.616H1C9LD41ema0XrvkpWdhbjkFsaEyEp_TqWvjMG9Ag.PNG.akj61300/new_prj01.png?type=w773" />

저의 경우는 Kotlin 이 익숙하기 때문에 Kotlin 을 선택하였지만 Android Things 를 하기위해 반드시 Kotlin 을 사용해아 하는 것은 아닙니다. Java 가 편한 경우 Java 로 프로그램을 작성해도 똑같은 일을 할 수가 있습니다.

다음 화면에서는 어플리케이션이 실행될 플랫폼을 고르는 화면입니다. Android Studio 3.0 부터는 Android Things 가 아예 IDE 차원에서 지원이됩니다. 플랫폼을 Phone 체크를 해제하고 Android Things 를 선택합니다.

<img src="http://postfiles2.naver.net/MjAxNzExMjZfMTc2/MDAxNTExNjQyNzMzNzQ0.BXXa11BRgoGLPrQMjOtoBInt8ms5Fp-lReSyLwCCFhYg.PXZ79Pv2zChBzkcpj_vMO_iRGPmGlfNLZlhYO61rDJIg.PNG.akj61300/new_prj02.png?type=w773" />

다음 화면에서는 Android Things Empty Activity 를 선택하고 그 다음 화면인 액티비티 이름 설정에서는 기본값을 그대로 선택하여 프로젝트 생성을 완료합니다.

<img src="http://postfiles6.naver.net/MjAxNzExMjZfMTQ2/MDAxNTExNjQzMTIzMTc1.SvenLWBNoQgmddQH1laxgNKo2_dwt6kQ0ydfZB3PLjIg.IfKaTh_sEY_M-Gr6rh9FGF5oLoJTp7bqjF6t2NjL__Ig.PNG.akj61300/new_prj03.png?type=w773" //>

Android Things 플랫폼이 Android Studio 에서 통합지원되면서 개발을 위해 해야할일은 대폭 줄어들었습니다. 예를 들면 기존에는 Android Things 개발을 위해 라이브러리 의존성을 build.gradle 에 추가하고 AndroidManifest.xml 을 수정하는 등 부가적인 작업들이 필요했습니다.

지금은 안드로이드 스튜디오가 그런 부가적인 것들을 자동으로 해주기 때문에 특별히 신경 쓸 필요는 없지만, 다른 안드로이드 모듈과 어떻게 다른지 알아보기 위해 차이점을 잠깐 살펴보도록 하겠습니다.

먼저 build.gradle 파일을 보도록 하겠습니다.

```groovy
dependencies {
    ...
    compileOnly 'com.google.android.things:androidthings:+'
    ...
}
```

build.gradle 파일에 android things 라이브러리가 의존성이 추가되어있습니다. compileOnly 는 기존에 provided 와 비슷합니다. Complile 때 클래스패스를 참조하지만 런타임시에는 class 파일을 포함하지 않습니다. 

그 이유는 android-things 라이브러리는 android-things 디바이스에 시스템 라이브러리로 존재하기 때문입니다. 굳이 어플리케이션이 라이브러리의 class 까지 포함할 필요가 없기 때문인 것이죠.

다음에 확인해볼 파일은 AndroidManifest.xml 파일 입니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.akj.firstandroidthings">

    <application>
        <uses-library android:name="com.google.android.things" />

        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.IOT_LAUNCHER" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

AndroidManifest.xml 파일에서 일반적인 안드로이드 프로젝트와 다른 부분은 uses-library 부분과 Intent Filter 부분입니다.

```xml
<uses-library android:name="com.google.android.things" />
```

uses-library 는 앱이 링크되어야 할 공유 라이브러리를 시스템에 알리는 역할을 합니다. 앞서 android-things 라이브러리는 compleOnly 로 작동한다고 하였습니다. 그렇기 때문에 런타임시에는 시스템에 android-things 라이브러리와 링크되어야 하기 때문입니다.

```xml
<category android:name="android.intent.category.IOT_LAUNCHER" />
```

인텐트 필터의 category 는 인텐트를 처리해야 하는 구성 요소의 종류에 관한 추가 정보를 담은 것입니다. 여기서는 IOT_LAUNCHER 이라는 추가정보를 가지고 있습니다. 

IOT 는 특성상 부팅후 하나의 앱이 자동으로 실행될 필요가 있습니다. 하나의 앱이 진입점(Entry Point) 으로서 작동되어야 하는것이죠. 

category.IOT_LAUNCHER 는 앱의 구성요소를 부팅시 자동으로 실행되는 진입점으로 설정하는 역할을 합니다. 

위 두가지 차이점이 Android Things 프로젝트를 결정짓는 사항입니다. 실제로 일반 안드로이드 프로젝트를 생성하고 build.gradle 에 라이브러리 의존성을 추가하고 AndroidManifest.xml 을 수정하면 Android Things 를 개발 할 수 있습니다.

이제 Android Things 프로젝트를 만들어봤으니 LED 를 제어하는 코드를 작성하도록 하겠습니다.

## LED 제어 코드 작성해보기

이제 MainActivty.kt 파일을 열고 LED 를 제어하는 코드를 작성해보겠습니다.

```kotlin
package com.akj.firstandroidthings

import android.app.Activity
import android.os.Bundle
import android.util.Log
import com.google.android.things.pio.Gpio
import com.google.android.things.pio.PeripheralManagerService
import java.io.IOException
import java.util.*
import kotlin.concurrent.schedule

class MainActivity : Activity() {

    val TAG = "MainActivity"

    /**
     * LED 를 On / Off 할 딜레이
     */
    val delay = 1000L

    /**
     * GPIO(General-purpose input/output) 의 이름.
     */
    val gpioName = "BCM6"

    /**
     * 안드로이드 타이머
     */
    val timer:Timer = Timer()
    val timerTask:TimerTask? = null

    /**
     * GPIO 인스턴스
     */
    var gpio: Gpio? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // PeripheralManagerService 는 주변장치 관리자 같은거
        val service = PeripheralManagerService()

        // gpio 현재 하드웨어에 존재하는 GPIO 리스트를 로깅
        Log.d(TAG, "Available GPIO: " + service.gpioList)

        try {
            gpio = service.openGpio(gpioName)

            // 입출력 설정을 체크한다.
            // DIRECTION_OUT_INITIALLY_LOW 는 GPIO 를 출력모드로 설정
            gpio?.setDirection(Gpio.DIRECTION_OUT_INITIALLY_LOW);

            // LED 를 ON/OFF 하는 타이머 시작
            timer.schedule(0, delay, {
                gpio?.let { gpio ->
                    // gpio 의 값을 현재값의 반대로 설정
                    gpio.value = !gpio.value
                }
            })

        } catch (e: IOException) {
            Log.e(TAG, "Error on PeripheralIO API", e);
        }
    }
}
```

코드에서 핵심부분은 42번째 라인부터입니다.

```kotlin
val service = PeripheralManagerService()
```

위 코드에서 Android Things 에 장치관리자를 가져옵니다.

```kotlin
gpio = service.openGpio(gpioName)
```

그리고 PeripheralManagerService 를 이용해 GPIO Pinout 객체를 가져오는 것이죠.

이제 프로그램을 실행하고 결과를 확인하도록 하겠습니다.

## 결과 확인해보기

프로그램이 실행되면 LED 가 1초마다 꺼졌다가 켜지게 됩니다. 

<img src="http://postfiles1.naver.net/MjAxNzExMjZfOTgg/MDAxNTExNjQ3MjgyMTQ3.dLGvcFJve4zdlSLj3t2mo_eXjT3Oizfe0Dgtv91svf0g.RO866aII8ihmj3s-CHP4fYzJIMoJJ6SXHY4JEHBrf4Qg.GIF.akj61300/android-things-result01.gif?type=w773" width="400px" />

Android Things 를 사용해서 간단한 LED 제어를 해보았습니다. 회로 연결이 익숙하다면, LED 제어 코드는 매우 짧기 때문에 특별한 어려움이 없을것으로 생각됩니다.

## 참고소스

이 문서에서 작동되는 코드는 github 에 공유되어 있습니다. 다음 github 주소를 참조해주세요.

[https://github.com/ahn-kj/android-things-study/tree/master/FirstAndroidThings](https://github.com/ahn-kj/android-things-study/tree/master/FirstAndroidThings)

## Next Step

다음 과정에서는 지금 만들어본 LED 제어를 Firebase 와 연동하여 웹에서 원격으로 제어하는 방법을 알아보고, 또 안드로이드 스마트폰에서 LED 제어를 할 수 있도록 프로그램을 구현해봅니다.