---
title: 'equals overriding 살펴보기!'
date: 2017-11-10 12:00:00
categories:
- css
tags:
- css
- selector
---

* 작성자 : 유현석

# equals overriding (이퀄즈 오버라이딩)

. 기본적으로 equals()라는 메소드는 자바의 기본 패키지인 java.lang패키지에 속해있다.
이 메소드는 기본적으로 객체끼리 비교를 할때 사용하는 메소드다.
그런데, 기본적으로 객체는 고유하기 때문에 같을 수가 없다. 
따라서 Card cd = new Card()라고 되어있는 것과 Card cd1 = new Card()라고 되어있는 것을 볼 경우에
cd.equals(cd1)는 false값을 가질 수 밖에 없다. 객체는 기본적으로 다르기 때문이다.
그렇다면 이 메소드를 사용할 이유가 없어진다. 
그럼 이 equals라는 메소드는 어떻게 사용되어 질까? 
이 물음에 대한 대답이 equals overriding으로 설명될 수 있을것 같다.

다시한번 위에 cd와 cd1을 돌아보자!
기본적으로 카드케이스를 만들어서 카드를 생성하게 되면 
프로그램 상으로의 구현은 아래와 같다. 
  Card cd1 = new Card();
  Card cd2 = new Card();
  Card cd3 = new Card();
  Card cd4 = new Card();
  Card cd5 = new Card();
  ...
. 이렇게 만들어진 카드들을 CardCase라는 객체에 넣어야 한다. 
. 그럼 이때 카드가 만들어 질때 cd1안에 "H8"(하트+숫자8)이라는 값이 생긴다.
이때 만들어진 카드가 랜덤으로 생성이 되기때문에 같은 카드가 만들어질 수 있다.
그래서 새로 생성한 카드안에 있는 카드의 값이 같으면 그 카드는 CardCase에
넣으면 안된다.

이럴때 cd1에 들어있는 값이 "H8"이고
새로 생성한 cd2에 들어있는 값이 "H8"일 경우에는 
같은 객체로 인식해서 객체를 만들지 않도록 할때 사용한다.

다시 정리하자면, 객체는 기본적으로 다르기때문에 equals()메소드로 객체를
비교하면 무조건 false가 나온다.
그런데 객체안의 값이 같으면 같은 객체라고 인식하게 해서 똑같은 카드가 
나오지 않도록 하는 것이 이퀄즈 오버라이딩이다.(equals overriding)

이퀄즈 오버라이딩을 하는 방법은 
. 해당 객체에서 equals(Object o)메소드를 오버라이딩 해주고
  class Card{
      private String card;
      ...
	  public boolean equals(Object o){
	      boolean isT = false;
	      Card cd = (Card)o;
	      if(card.equasl(cd.getCard()){
	         isT = true;
	      }
	      return isT;
	  }  
      ...
	  public String getCard(){
    	  return card;
	  }
  }


. 해쉬코드까지 오버라이딩을 해주면 된다. 
(단, 해쉬코드 오버라이딩 할때 숫자를 더해줘야 하는데 그 수는 솟수이다.)
  public int hashCode(){
     retrun super.hashCode()+137;
  }

이렇게 객체안에 존재하는 값이 같으면 객체가 같다고 할때
이퀄즈 오버라이딩을 사용하면 된다.
이상 equalse overring에 대한 간단한 설명을 마칩니다.^^
