---
title: Anaconda와 Jupyter를 이용한 파이썬 실행환경 구축 해보기
date : 2017-11-26 12:00:00
author : 황희정
---



# Anaconda와 Jupyter를 이용한 파이썬 실행환경 구축 해보기

## 아나콘다(Anaconda)란?

Anaconda는 세계에서 가장 유명한 파이썬(Python) 데이터 과학 플랫폼이다. 한번의 클릭으로 모든데이터 과학 패키지를 쉽게 설치하고 패키지, 종속성 및환경을 관리할 수 있다.

![anaconda_feature1](http://tech.javacafe.io/img/blog/20171126/hwang_20171126_1.png)



![anaconda_feature2](http://tech.javacafe.io/img/blog/20171126/hwang_20171126_2.png)



### 1. 실행 전 준비사항

* Python 설치



### 2. 아나콘다(Anacoda 설치)

* https://www.anaconda.com/download/ 에서 다운로드 후 설치파일 실행

  (설치 동의 후, 계속 및 설치만 누르면 되므로 설치과정 생략)

  ![anacondainstall](http://tech.javacafe.io/img/blog/20171126/hwang_20171126_3.png)



* 설치된 후, 보이는 화면

![anacondascreenshot](http://tech.javacafe.io/img/blog/20171126/hwang_20171126_4.png)

- Home에서 설치된 패키지 확인 가능

- Envorinments : 추가적으로 패키지 추가 가능

  ​



### 3. (예시) tensorFlow 환경 구축

* Environment탭에서 오른쪽 탭에서 not installed 를 선택 한 후, tensorflow 검색 후, Apply 적용)

![tensorflowinstall](http://tech.javacafe.io/img/blog/20171126/hwang_20171126_9.png)





### 4. (예시) Jupyter Notebook 으로 실행해보기

### Jupyter Notebook 이란?

Project Jupyter는 수십 개의 프로그래밍 언어에서 오픈 소스 소프트웨어, 개방형 표준 및 대화식 컴퓨팅을위한 서비스를 지원한다.

![jupyterinfo](http://tech.javacafe.io/img/blog/20171126/hwang_20171126_5.png)



### Anaconda에서 Jupyter Notebook 사용

*  Anaconda Home에서 Jupyter Launch 버튼 클릭하면, 로컬에 브라우저 콘솔창(?)이 실행된다.

![anacondascreenshot](http://tech.javacafe.io/img/blog/20171126/hwang_20171126_4.png)



![jupyter](http://tech.javacafe.io/img/blog/20171126/hwang_20171126_6.png)



* New 버튼을 클릭한다.

  ![jypyter](http://tech.javacafe.io/img/blog/20171126/hwang_20171126_7.png)

* 커멘드 실행해보기

  * In 쪽에 명령어를 입력하고, ▷ 버튼을 누르면, 결과가 출력된다.

  ![jypytercommand](http://tech.javacafe.io/img/blog/20171126/hwang_20171126_8.png)