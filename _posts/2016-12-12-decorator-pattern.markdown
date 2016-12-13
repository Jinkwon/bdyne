---
layout: post
title:  "Decorator Pattern"
date:   2016-11-22 02:19:11 +0900
categories: jekyll update
---

데코레이터 패턴은 OCP(Open Closed Principle) 를 빼놓고는 이야기를 할 수가 없습니다.  
디자인 원칙에서 가장 중요하다고 여겨지는 원칙 가운데 하나죠.

### 개방-폐쇄 원칙(OCP, Open-Closed Principle)
'소프트웨어 개체(클래스, 모듈, 함수 등등)는 확장에 대해 열려 있어야 하고, 수정에 대해서는 닫혀 있어야 한다.'는 프로그래밍 원칙이다.

[위키페디아 참고](https://ko.wikipedia.org/wiki/%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84_%EC%9B%90%EC%B9%99)


클래스를 작성할 때 한개의 클래스를 수정한다고 했을 때 이를 참조하고 있는 수많은 클래스가 있다면,
그에 따른 사이드 이펙트 (Side Effect) 방지에 대해 보장할 수 없고 그 프로그램은 신뢰성을 잃게 됩니다.   
테스트 코드를 유지하고 있다면 그에 따른 코드까지 연쇄적으로 수정해 주어야 합니다.  
  
이 원칙에 따라 원 클래스는 수정하지 않으면서도 새로운 기능을 부여하는 디자인 패턴이 바로 데코레이터 패턴입니다.  


## 데코레이터 패턴

![decorator screenshot]({{ site.url }}/images/attachment/decorators.jpg)

이것이 데코레이터 패턴의 일종의 모형이라고 볼수가 있습니다.  
Component라는 인터페이스가 있으며 이걸 구현하는 구상클래스로는 Decorator 클래스와  ConcreteComponent 클래스가 있습니다.   
Decorator 클래스에는 Component 객체를 구성요소로 가지고 있다는 표시도 되어있습니다.   
Decorator의 구상클래스로 또 두개가 있는것을 볼 수가 있습니다.  

추가적인 요건들을 동적으로 첨가한다는 의미는 역시나 Decorator 클래스에 Component의 객체가 구성 요소로 있기 때문인것 같고, 
기능을 유연하게 확장한다는 의미는 데코레이터의 구상 클래스를 추가 시켜주는 것 만으로 상위 클래스나 다른 클래스의 변경이 없기 때문이라고 볼 수 있습니다.  


## 자바스크립트를 이용한 데코레이터 구현 예제

```javascript
//Class to be decorated
function Coffee(){
    this.cost = function(){
        return 1;
    };
}
 
//Decorator A
function Milk(coffee){
    this.cost = function(){
        return coffee.cost() + 0.5;
    };  
}
 
//Decorator B
function Whip(coffee){
    this.cost = function(){
        return coffee.cost() + 0.7;
    };
}
 
//Decorator C
function Sprinkles(coffee){
    this.cost = function(){
        return coffee.cost() + 0.2;
    };
}
 
//Here's one way of using it
var coffee = new Milk(new Whip(new Sprinkles(new Coffee())));
alert( coffee.cost() );
 
//Here's another
var coffee = new Coffee();
coffee = new Sprinkles(coffee);
coffee = new Whip(coffee);
coffee = new Milk(coffee);

coffee.cost();
```


## 근데, 상속과 뭐가 다른가?
데코레이터 패턴을 보다보면 이런 생각이 떠오릅니다. 그래서 상속이 무엇이 다른지?  
저 역시 처음 데코레이터 패턴을 봤을 때 비슷한 생각을 했습니다. ~~상속을 하면 되잖아요!!~~  
차이점을 두 단어로 표현하면, '동적과 정적' 으로 표현할 수 있습니다.  
상속을 이용하면 이미 정적으로 인터프리팅(혹은 컴파일) 시점에 그 기능이 이미 결합되어 버립니다.   
데코레이터 패턴을 이용하면 런타임에 유연하게(동적으로) 이를 적용할 수 있게 됩니다.    


## 데코레이터 패턴의 단점

역시 수정할 수 없는 (코드 변경에 닫혀있는) 클래스를 가지고 여러가지 파일을 만들다 보면 그만큼 많은 파일이 생기기 마련입니다.  
데코레이터 패턴의 가장 큰 단점이라고 하지만 이 역시 본 패턴의 특징이고 이를 잘 이용하는 것이 좋겠습니다.


## 마치며

OCP는 디자인 원칙에서 지켜야 할 원칙은 맞지만 무조건 OCP를 적용할 필요는 없습니다.  
쉽게 갈 수 있는 길임에도 원칙 때문에 너무 복잡한 길을 돌아갈 필요는 없습니다.  
코드의 세계에서는 언제나 중도가 필요하기 마련이고 부득이하게 코드를 변경해야 한다면 변경하는게 더 좋을 수도 있습니다.  
우리가 디자인 패턴을 공부하고 사용하는 것은 이해하기 쉽고 알기 쉽게 코드를 작성하기 위함이라는 것은 잊지 말아야 겠습니다.  