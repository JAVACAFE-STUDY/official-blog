---
title: Intellij JSON Fragment Edit 기능 
date : 2017-12-19 11:37:00
author : 이수연
---

Intellij JSON Fragment Edit 기능
================================

> Intellij 사용시 JSON을 편집 할 수 있는 **JSON Fragment Edit 기능** 제공


1. JSON문자열을 넣을 String변수 선언. <br>
![](http://tech.javacafe.io/img/blog/20171219/tip_01.png)

2. 변수의 값이 되는 **""** 사이에 커서를 두고, **alt+enter**를 누르면 **Inject language or reference 선택** 시 아래와 같이 선택 리스트 생성 됨. <br>
![](http://tech.javacafe.io/img/blog/20171219/tip_02.png)

3. JSON 선택 시 아래와 같은 팝업 생성됨, 한번 더 **alt+enter**. <br>
![](http://tech.javacafe.io/img/blog/20171219/tip_03.png)

4. **//language=JSON** 라는 주석이 생성. <br>
![](http://tech.javacafe.io/img/blog/20171219/tip_04.png)

5. 다시 **""** 사이에 커서를 두고, **alt+enter**를 누르면 메뉴가 나오는데 이때 **Edit JSON Fragment** 선택. <br>
![](http://tech.javacafe.io/img/blog/20171219/tip_05.png)

6. 아래 이미지와 같은 JSON편집 화면 생성 됨. <br>
![](http://tech.javacafe.io/img/blog/20171219/tip_06.png)

7. Fragment 편집 화면에서 JSON을 직접 편집하면 편집한 내용이 자동으로 변수에 값 변환이 되어서 적용 됨. <br>
![](http://tech.javacafe.io/img/blog/20171219/tip_07.png)

---

> Inject Ianguage or reference 가 보이지 않을 때

![](http://tech.javacafe.io/img/blog/20171219/tip_08.png)


1. command+enter -> Editor -> Intentions 선택. <br>
![](http://tech.javacafe.io/img/blog/20171219/tip_09.png)

2. Language Injection 리스트 내에 있는 Inject language or reference 클릭 저장. <br>
![](http://tech.javacafe.io/img/blog/20171219/tip_10.png)

3. Inject language or reference 선택 팝업 정상 생성. <br>
![](http://tech.javacafe.io/img/blog/20171219/tip_11.png)
