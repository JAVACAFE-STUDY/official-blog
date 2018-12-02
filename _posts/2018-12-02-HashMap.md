---
title : 자바 HashMap을 효과적으로 사용하는 방법
date : 2018-12-02 18:20:30

author : 이정근
--- 

HashMap 은 편하고 빠르다. 하지만 어떻게 하면 효율적으로 잘 사용할지 몸부림치는 순간도 많다.

원문은 https://dzone.com/articles/how-to-use-java-hashmap-effectively 이다.



`HashMap` 는 자바 개발자가 거의 매일 사용하는 가장 유명한 데이터 구조 중 하나이다.

이번 글에서는 자바에서 `HashMap` 이 무엇인지, 왜 사용해야하는지, 어떻게 사용할지에 대하여 다루려 한다. `HashMap` 기술에 깊히 빠져들기전에 예를 들어보자.

여러분이 편의점(원문은 식료품점)을 운영하고 있고, 여러분의 가게에서 많은 종류의 상품을 다루고 있다고 하자. 그 많은 상품들은 각자 이름과 가격이 있다. 이 모든 상품을 모두 기억하고 있는 것은 어려운 일이다. 상품들의 정보를 노트에 기록하는 중에도 상품을 팔아야되며 또 그것들의 가격을 찾는 등의 행위를 해야한다.

만약에 노트에 이름순으로 적어 놓지 않는다면 매번 상품에 대한 정보를 찾는 시간이 오래걸릴 것이다. 알고리즘적으로 표현하자면 O(n)의 복잡도를 가진다고 한다. 하지만 이름순으로 유지가된다면 이진탐색을 할수가 있으므로 (log n)의 시간에 찾기가 가능하다. 즉 O(log n)이다. 알다시피 O(log n)은 O(n)보다 적게 시간이 걸린다.

비록 O(log n) 이 상당히 적은 시간이 소요된다 하더라도 여전히 어느 정도의 시간은 걸린다. 시간이 전혀 걸리지 않는다면 그것이 최선일 것이다. 아마 여러분이 모든 상품의 이름과 가격을 기억할 수 있고 손님이 상품의 이름을 말하는데로 바로 가격을 대답할 수 있는 경우일 것이다. 

이 경우에 대한 정확한 해결방법이 `HashMap` 을 사용하는 것이다. 상품의 이름과 그것의 가격을 `HashMap` 에 넣고 거의 시간이 걸리지 않고 key로써 value를 얻을 수 있다.

이제, `HashMap` 에 대하여 정의 하자. `HaskMap`  은 고정시간을 제공하는 key-value 데이터 구조이다. O(1) 복잡도는 get과 push 동작에 동일하다.

아래 예를 보자

```java
// java 11에서 map을 만드는 방법
var productPrice = new HashMap<String, Double>();

//java 8의 경우
Map<String, Double> productPrice = new HashMap<>();

// add value
productPrice.put("Rice", 6.9);
productPrice.put("Flour", 3.9);
productPrice.put("Sugar", 4.9);
productPrice.put("Milk", 3.9);
productPrice.put("Egg", 1.9);
```

이제, 가격을 알기 위하여 map에서 key(상품의 이름)이 사용한다.  `get ()` 메소드를 사용한다.

```java
//get value
Double egg = productPrice.get("Egg");
System.out.println("The price of Egg is: " + egg);
```

`HashMap`  에 대하여 기본적인 것을 다루었으니 좀 더 깊게 들어가서 사용법을 알아보자.



## **HashMap에서 모든 Key와 Value를 print하기** 

HashMap에서 모든 Key와 Value를 print하는 다양한 방법이 있다. 하나씩 알아 보자.

1. 모든 key를 print 하는 방법

```java
Set<String> keys = productPrice.keySet();

//print all the keys 
for (String key : keys) {
  System.out.println(key);
}

// or
keys.forEach(key -> System.out.println(key));
```

좀 더 간결한 람다 표현식인 `forEach` 를 선호한다.



2. 모든 value를 print 하는 방법

```java
Collection<Double> values = productPrice.values();
values.forEach(value -> System.out.println(value));
```



3. 모든 key와 value를 같이 print하는 방법

```java
Set<Map.Entry<String, Double>> entries = productPrice.entrySet();

for (Map.Entry<String, Double> entry : entries) {
  System.out.print("key: "+ entry.getKey());
  System.out.println(", Value: "+ entry.getValue());
}

// or (lambda expression)
productPrice.forEach((key, value) -> {
  System.out.print("key: "+ key);
  System.out.println(", Value: "+ value);
});
```



## **Key가 존재하는지 알고 싶은 경우** 

종종, key가 존재하는지 아닌지 확인하고 싶은 경우가 생긴다. 이 정보를 파당으로 분기를 만들어야 한다. 예를 들어 동적프로그램에서 [메모이제이션](https://ko.wikipedia.org/wiki/%EB%A9%94%EB%AA%A8%EC%9D%B4%EC%A0%9C%EC%9D%B4%EC%85%98)(Memoization, 역주: 원문의 memorization는 오타인듯) 은 가장 자주 사용되는 기술 중에 하나이다. 예제를 보자.

피보나치 수열은 재귀를 보여주는 경우에 유명한 수열이다. 이 수열의 공식은 아래와 같다.

```
f(n) = f(n-1) + f(n-2)
```

그러나 이번 예제에는 같은 값이 계속해서 계산될 것이다. 메모이제이션을 사용한다면 쉽게 예방할 수 있다. 

```java
import java.math.BigInteger;
import java.util.HashMap;
import java.util.Map;
public class Fibonacci {
    private Map<Integer, BigInteger> memoizeHashMap = new HashMap<>();
    {
        memoizeHashMap.put(0, BigInteger.ZERO);
        memoizeHashMap.put(1, BigInteger.ONE);
        memoizeHashMap.put(2, BigInteger.ONE);
    }
    private BigInteger fibonacci(int n) {
        if (memoizeHashMap.containsKey(n)) {
            return memoizeHashMap.get(n);
        } else {
            BigInteger result = fibonacci(n - 1).add(fibonacci(n - 2));
            memoizeHashMap.put(n, result);
            return result;
        }
    }
    public static void main(String[] args) {
        Fibonacci fibonacci = new Fibonacci();
        for (int i = 0; i < 100; i++) {
            System.out.println(fibonacci.fibonacci(i));
        }
    }
}
```

코드에서 피보나치 번호가 이미 `memoizeHashMap`  에 있는지 여부를 체크한다. 만일 이미 있다면 , 계산할 필요가 없고 단지 key를 가지고 get을 하면 된다. 아닌 경우에는 계산한다.

위의 코드에서는 100 까지의 피보나치 번호를 빠르게 제공한다.

Note:  map에 해당 key가 존재한다면  `put()` 메소드가 value를 key로 대체한다.

우리의 삶을 편하게 만드는 `HashMap` 에서 사용가능한 메소드 몇개를 살펴보자.



## 비교: computeIfAbsent() 과 puIfAbsent()

앞서의 코드블록에서 `finonacci()` 메소드를 다시 살펴보자.  `containsKey()` 대신에  `computeIfAbsent()`  메소드를 사용한다면 우리는 이것을 좀 더 짧고 간결하게 만들 수 있다. 

```java
private BigInteger fibonacci (int n) {
  return memoizeHashMap.computeIfAbsent(n, 
           (key) -> fibonacci(n - 1).add(fibonacci(n - 2)));
}
```

이 메소드는 두개의 입력값을 받는다. 첫번째는 key이고 두번째는 key를 사용하여 차례데로  value를 반환하는 함수형 인터페이스이다. map에 key가 있으면 값을 반환한다는 아이디어이다. 그렇지 않다면, value를 계산하고 map에 추가를 한후에 value를 돌려줄것이다. 이렇게 하면 코드가 보다 간단해 지고 짧아진다.

그러나 value를 직접 얻을 수 있는  `putIfAbsent()`  라는 다른 메소드도 있다. 

```
productPrice.putIfAbsent("Fish", 4.5);
```



### putIfAbsent() 와 computeIfAbsent() 의 다른 점

`computeIfAbsent()` 는 만일 키가 없으면 값을 얻기 위하여 호출하는 매핑된 함수를 가지는 반면,  `putIfAbsent()`  는 value를 바로 가진다. 그러므로 key의 value는 메소드 호출에서 오고 만일 메소드가 비싸다면(계산이 비용이 많이든다면) `putIfAbsent()` 는 무조건 계산을 하지만  `computeIfAbsent()` 는 key를 찾을 수 없지 않는 한 계산을 하지 않는다. 

```java
var theKey = "Fish";        

// key가 존재하여도 callExpensiveMethodToFindValue()가 호출된다.
productPriceMap.putIfAbsent(theKey, callExpensiveMethodToFindValue(theKey)); 

// key가 존재한다면 callExpensiveMethodToFindValue()가 결코 호출되지 않는다. 
productPriceMap.computeIfAbsent(theKey, key -> callExpensiveMethodToFindValue(key));
```



## **비교 : compute() 와 computeIfPresent()** 

비슷하게, `HashMap` 은 `compute()` 와 `computeIfPresent()`  메소드가 있다. 

알다시피, 오라클이 Java EE를 이클립스제단에 기부하였고 결과적으로 Jakarta EE로 이름이 바뀌었다. (https://www.infoq.com/news/2018/02/from-javaee-to-jakartaee). 이 주제에 대한 글을 수집하고 이와 관련된 단어를 사람들이 얼마나 선택하고 자주 언급하는지 계산하는 프로그램을 작성해보자.

```java
import java.util.HashMap;
import java.util.Map;
public class WordFrequencyFinder {
    private Map<String, Integer> map = new HashMap<>();
    {
        map.put("Java", 0);
        map.put("Jakarta", 0);
        map.put("Eclipse", 0);
    }
    public void read(String text) {
        for (String word : text.split(" ")) {
            if (map.containsKey(word)) {
                Integer value = map.get(word);
                map.put(word, ++value);
            }
        }
    }
}
```

위의 코드는 너무 간단하다. 첫째로 key가 map에 이미 존재하는지 확인한다. 존재한다면 value를 자져와서 1증가시킨 value로 업데이트한다. 이것은 구식의 방법이다.

Java 8 은  `computeIfPresent()` 라는 멋있는 메소드를 제공한다. 이 메소드로 다시 작성해보자.

```java
public void read(String text) {
  for (String word : text.split(" ")) {
    map.computeIfPresent(word, (String key, Integer value) -> ++value);
  }
}
```

위의 코드는 보다 멋있어졌다.   `computeIfPresent()` 메소드는 key를 받고 만일 key가 존재하는 경우에만 반복적으로 value 를 계산하는 함수로 다시 매핑한다.  그러므로 다시 매핑한 함수는 map에 오직 key가 존재하는 경우에만 호출되고 그 반에의 경우에는 호출하지 않는다.

게다가,  `computeIfPresent()` 와 비슷한 입력값을 가지는  `compute()` 이라는 다른 메소드가 있다. 그러나 이 메소드는 재매핑된 함수로 계산을 하고 key가 이미 존재하는지 여부에 상관하지 않는다. 다시 매핑된 함수의 출력값을 바탕으로 map에 value를 추가한다.



## **getOrDefault()**

일부의 경우에, 우리가 찾는 key를 가지지 않는 map이 있을 수 있다. 그러나 여전히 value를 가지기 원하고 map이 변경되지 않기를 원하는 경우가 있다. 이런 경우에 `getOrDefault()`  메소드를 사용할 수 있는데, 매우 유용하다.

```java
productPriceMap.getOrDefault("Fish", 29.4);
```



## 마무리

`HashMap` 은 데이터에 쉽고 빠르게 접근하도록 해준다. 그리고  Java 8 과  lambda 표현식의 도입으로 개발자로서의 삶을 보다 더 심플하게 만들어 준다. 우리는 우리의 코드를 보다 짧고 간결하게 만들수 있는 람다 표현식을 사용할 수 있다. 

