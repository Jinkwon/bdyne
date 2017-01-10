---
layout: post
title:  "이터레이터 패턴 (Iterator Pattern)"
date:   2017-01-10 00:00:00 +0900
categories: design pattern
---


리스트같은 집합 객체(데이터 군)들의 내부표현 구조들을 노출시키지 않고 원소들을 반복자(Iterator)를 이용하여 접근하는 패턴입니다.
이 패턴을 이용하면 집합체 내에서 어떤 식으로 일이 처리되는지에 대해서 전혀 모르는 상태에서 그 안에 들어있는 모든 항목들에 대해서 반복작업을 수행할 수 있습니다.



복합 객체 요소들의 내부 표현 방식은 공개하지 않고 순차적인 접근 방법을 제공하는 패턴.

### Iterator 패턴의 구성

- Iterator
- ConcreteIterator
- Aggregate
- ConcreteAggregate

![screenshot]({{ site.url }}/images/attachment/iterator-pattern.png)

1. Aggregate 인터페이스 - 집합체를 나타내는 인터페이스
2. Iterator 인터페이스 - 하나씩 나열하면서 검색을 실행하는 인터페이스
3. Book 클래스 - 책을 나타내는 클래스
4. BookShelf 클래스 - 서가를 나타내는 클래스
5. BookShelfIterator 클래스 - 서가를 검색하는 클래스
6. Main 클래스 - 동작 테스트용 클래스


### Iterator 패턴을 사용하는 이유
이터레이터 패턴을 이용하면 내부적인 구현 방법을 외부로 노출시키지 않으면서도 집합체에 있는 모든 항목에 일일이 접근할 수 있습니다.
또한 각 항목에 일일이 접근할 수 있게 해 주는 기능을 집합체가 아닌 반복자 객체에서 책임지게 된다는 것도 장점으로 작용합니다.
그러면 집합체 인터페이스 및 구현이 간단해지고 각자 중요한 일만 잘 처리할 수 있으면 되니까요.


### Iterator in JS
ES5 에서는 배열을 순회하기 위해서는 아래와 같은 형태로 이용하곤 했습니다.

```javascript
for (var index = 0; index < myArray.length; index++) {
  console.log(myArray[index]);
}
```

ES5 발표 이후에는 forEach 메소드를 쓸 수 있게 되었지만 iterator 개념처럼 완벽하게 이터레이터의 기능을 이용할 수는 없습니다.

```javascript
myArray.forEach(function (value) {
  console.log(value);
});
```


##### for-of Loop
ES2015부터 iterator 기능이 추가되었으며 아래와 같은 형태로 이용이 가능합니다.

```javascript
let iterable = [1, 2, 3];
for (let item of iterable) {
    console.log(item); // 1, 2, 3
}

let iterable2 = new Set([4, 5, 6]);
for (let item of iterable2) {
    console.log(item); // 4, 5, 6
}

let iterable3 = '789';
for (let item of iterable3) {
    console.log(item); // '7', '8', '9'
}
```

- Low-level iterator consumption

```javascript
let iterable = ['a', 'b', 'c'];

// Explicit "low-level" iterator consumption:
let iterator = iterable[Symbol.iterator]();
iterator.next(); // { value: 'a', done: false }
iterator.next(); // { value: 'b', done: false }
iterator.next(); // { value: 'c', done: false }
iterator.next(); // { value: undefined, done: true }

```


- 내용 발췌 : Head First Design Pattern
