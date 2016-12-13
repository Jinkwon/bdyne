---
layout: post
title:  "Singleton Pattern"
date:   2016-11-22 02:19:11 +0900
categories: jekyll update
---

## 싱글턴 패턴 (singleton pattern)
소프트웨어를 디자인 하다보면 하나만 있으면 되는 객체들이 있습니다.  
어플리케이션의 생명주기 안에서 단 하나의 클래스에 단 하나의 인스턴스만 생성하는 것을 싱글톤 패턴이라고 합니다.  
유틸과 같은 공통으로 사용하는 클래스에 적용하면 알맞을 것입니다.    
   
해당 클래스의 인스턴스가 하나만 만들어지고, 어디서든지 그 인스턴스에 접근할 수 있도록 하기 위한 패턴.
  

## 싱글톤 패턴의 특징
### 생성자가 없습니다.  
new 키워드를 통해 클래스를 생성하는 것이 아니라 getInstance 라는 형태의 메서드를 통해 객체 인스턴스를 가져옵니다.    

##### 고전적인 싱글톤 패턴 구현법
```java
class Singleton { 
    private static Singleton instance;
    private Singleton(){ 
    } 
    public static Singleton getInstance(){ 
        if (instance == null) 
            instance = new Singleton();  
        return instance; 
    } 
}
```

### Only One 인스턴스  
해당 함수의 최초 호출 시점에 자동으로 인스턴스가 생성되고 그 인스턴스를 전달합니다.

##### ClassLoader에 의해 미리 인스턴스를 생성하는 방법
```java
class Singleton { 
    private static Singleton instance = new Singleton(); 
    private Singleton(){} 
    public static Singleton getInstance(){ 
        if (instance == null) 
            instance = new Singleton(); 
        return instance; 
    } 
}
```

### 단점 
싱글턴 패턴을 사용할 때 주의해야 할 것은, 패턴의 특성상 어플리케이션 전체 범위에 영향을 주는 일종의 상태 정보가 생긴다는 것입니다.  
참고로 전역 변수를 사용할 때의 단점은 프로그램이 시작할 때부터 자원을 차지하고 들어가기 때문에 간혹 자원을 불필요하게 많이 사용할 가능성이 있습니다.

### 마치며
하나의 패턴은 다양한 형태로 표현되는 경우가 많은데, 경우에 따라 경계가 모호한 형태로 패턴이 구현되는 경우도 있습니다.  
이번 싱글톤 패턴은 목적과 형태가 분명한 패턴으로 항상 머리에 넣어두고 사용해야할 중요한 패턴이라고 말할 수 있습니다.   