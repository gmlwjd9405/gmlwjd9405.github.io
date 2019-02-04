---
layout: post
title: '[Java] == equals() compareTo() 차이와 사용법'
subtitle: '== equals compareTo 의 차이점을 이해하고 사용할 수 있다.'
date: 2018-10-06
author: heejeong Kwon
cover: '/images/java-programming/equals-compareto-main.png'
tags: java equals compareTo
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - ==의 개념을 이해한다.
> - equals()의 개념을 이해한다.
> - compareTo()의 개념을 이해한다.
> - == equals() compareTo()의 차이를 이해하고 사용할 수 있다.

## ==
* 항등 **연산자(Operator)** 이다.
  * <-->  !=
* <span style="background-color: #e1e1e1">참조 비교(Reference Comparison)</span>; (주소 비교, Address Comparison)
  * 두 객체가 같은 메모리 공간을 가리키는지 확인한다.
* 반환 형태: boolean type
  * 같은 주소면 return true, 다른 주소면 return false
* 모든 기본 유형(Primitive Types)에 대해 적용할 수 있다.
  * byte, short, char, int, float, double, boolean
  * 예시)
~~~java
// == operator for compatible data types 
class Test { 
    public static void main(String[] args) { 
        // integer-type 
        System.out.println(10 == 20); // false
        // char-type 
        System.out.println('a' == 'b'); // false
        // char and double type 
        System.out.println('a' == 97.0); // true
        // boolean type 
        System.out.println(true == true); // true
    } 
} 
~~~
* 객체 유형(Object Types)에 대해서도 적용할 수 있다.
  * 이때, 넘어온 객체 인자의 유형 간에 호환성이 있어야 한다.
    * Ex) 부모-자식 관계, 동일한 유형
  * 호환성이 없는 경우에는 컴파일 오류가 발생한다.
  * 예시)
~~~java
// == operator for incompatible data types 
class Test { 
    public static void main(String[] args) { 
        // 모두 다른 객체 
        Thread t = new Thread(); 
        Object o = new Object(); 
        String s = new String("HELLO"); 
        /* --print-- */
        System.out.println(t == o); // false
        System.out.println(o == s); // false
       // Uncomment to see error  
       System.out.println(t == s); // [compile time error]: incomparable types: Thread and String
    } 
} 
~~~

## equals()
* 객체 비교 **메서드(Method)** 이다.
  * <-->  !(s1.equals(s2));
* <span style="background-color: #e1e1e1">내용 비교(Content Comparison)</span>
  * 두 객체의 값이 같은지 확인한다.
  * 즉, 문자열의 데이터/내용을 기반으로 비교한다.
* 기본 유형(Primitive Types)에 대해서는 적용할 수 없다.
* 반환 형태: boolean type
  * 같은 내용이면 return true, 다른 내용이면 return false
* 예시)
~~~java
// Java program to understand  
// the concept of == operator 
public class Test { 
    public static void main(String[] args) { 
        String s1 = new String("HELLO"); 
        String s2 = new String("HELLO"); 
        Thread s3 = s1; // 같은 대상을 가리킨다.
        String s4 = new String("WORLD"); 
        /* --print-- */
        System.out.println(s1.equals(s2)); // true
        System.out.println(s1.equals(s3)); // true
        System.out.println(s3.equals(s4)); // false
    } 
} 
~~~

## == VS equals()
![](/images/java-programming/equals-example.png)
~~~java
public class Test { 
    public static void main(String[] args) { 
        // Thread 객체 
        Thread t1 = new Thread(); 
        Thread t2 = new Thread(); // 새로운 객체 생성. 즉, s1과 다른 객체. 
        Thread t3 = t1; // 같은 대상을 가리킨다.
        // String 객체 
        String s1 = new String("WORLD"); 
        String s2 = new String("WORLD"); 
        /* --print-- */
        System.out.println(t1 == t3); // true
        System.out.println(t1 == t2); // false(서로 다른 객체이므로 별도의 주소를 갖는다.)
        System.out.println(t1.equals(t2)); // false
        System.out.println(s1.equals(s2)); // true(모두 "WORLD"라는 동일한 내용을 갖는다.)
    } 
} 
~~~

## compareTo()
* `Interface Comparable<T>` 가 구현되어 있는 객체에서 사용가능한 객체 비교 **메서드(Method)**
  * Java에서 제공되는 정렬이 가능한 클래스들은 모두 Comparable 인터페이스를 구현하고 있으며, 정렬 시에 이에 맞게 정렬이 수행된다.
    ~~~java
    // Integer class
    public final class Integer extends Number implements Comparable<Integer> { ... }
    // String class
    public final class String implements java.io.Serializable, Comparable<String>, CharSequence { ... }
    ~~~
  * Ex) Integer, Double 클래스: 오름차순 정렬
  * Ex) String 클래스: 사전순 정렬
* 기본 유형(Primitive Types)에 대해서는 적용할 수 없다.
* 반환 형태: integer type
  * 현재 객체 < 인자로 넘어온 객체: return 음수 
  * 현재 객체 == 인자로 넘어온 객체: return 0 
  * 현재 객체 > 인자로 넘어온 객체: return 양수 
* **사용자가 정의한 정렬 기준에 맞춰 정렬하기 위한 용도** 로 compareTo() 메서드를 오버라이드하여 구현한다.
  * 구현 방법
    * 정렬할 객체에 Comparable interface를 implements 후, compareTo() 메서드를 오버라이드하여 구현한다.
  * compareTo() 메서드 작성법
    * 위의 반환 형태 참고
    * 음수 또는 0이면 객체의 자리가 그대로 유지되며, 양수인 경우에는 두 객체의 자리가 바뀐다.
  * 정렬 사용 방법
    * Arrays.sort(array)
    * Collections.sort(list)


# 관련된 Post
* Java의 Comparable, Comparator의 차이를 알고 싶으시면 [Java 객체 정렬 방법](https://gmlwjd9405.github.io/2018/09/06/java-comparable-and-comparator.html)을 참고하시기 바랍니다.


# References
> - [https://www.geeksforgeeks.org/difference-equals-method-java/](https://www.geeksforgeeks.org/difference-equals-method-java/)
> - [https://www.leepoint.net/data/expressions/22compareobjects.html](https://www.leepoint.net/data/expressions/22compareobjects.html)