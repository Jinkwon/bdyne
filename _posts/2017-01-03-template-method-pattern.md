---
layout: post
title:  "템플릿 메소드 패턴 (template method pattern)"
date:   2017-01-03 13:50:00 +0900
categories: design pattern
---

템플릿 메소드 패턴은 어떤 작업 알고리즘의 골격을 정의합니다. 일부 단계는 서브 클래스에서 구현하도록 할 수 있습니다.  
상위 클래스에서는 처리할 구조를 결정하고 하위 클래스에서 구체적인 내용을 정의하는 디자인 패턴을 말합니다.  
하위 클래스에서 어떤 구현을 해도 처리의 실제 큰 흐름은 상위 클래스가 결정한대로 정의를 해야 됩니다.  
템플릿 메소드를 이용하면 알고리즘 구조는 그대로 유지하면서 특정 단계만 서브 클래스에서 새로 정의하도록 할 수 있습니다.  


![template-method screenshot]({{ site.url }}/images/attachment/template-method.png)

### Template 사용을 하면,
상위 클래스에서는 하위 클래스에서 메소드가 구현되길 기대하고,  
다른 말로 하위 클래스에게 해당 메소드의 구현을 요청하는 형태가 됩니다.  
이를 Sub class responsibility 라고 지칭합니다.  

## 예제 개발 해 보기 (with Javascript)

#### 개발순서
1. 서브클래스간에 공통된 알고리즘을 확인하고 이 부분을 추상화 합니다.
2. 추상클래스에서는 공통 알고리즘을 템플릿 메소드로 구현합니다.
3. 알고리즘 메소드를 추상 클래스에서 구현 또는 추상메소드화 하여 서브클래스에서 재정의 할 수 있도록 구성합니다. 

#### 공통 추상 클래스 구현
```javascript
class HambergerMaker {
    constructor() {

    }

    create() {
        // template method pattern
        // 서브 클래스에 prepare와 addPatty 함수를 구현하게 한다.
        this.prepare();
        this.addPatty();

        /// 본 클래스에서는 공통으로 쓸 함수를 구현해 둔다.
        this.splitBread();
    }

    splitBread() {
        this.bread.split();
    }

    // js에는 abstract 가 없으므로 throw 로 유사하게 구현했음
    prepare() {
        throw 'prepare 메서드가 구현되어 있지 않습니다. 구현이 필요합니다.';
    }

    addPatty() {
        throw 'prepare 메서드가 구현되어 있지 않습니다. 구현이 필요합니다.';
    }
}
```

#### 서브 클래스 구현
이제 햄버거메이커를 상속받는 군데리아 클래스를 구현해 보겠습니다.  
부모 클래스에 존재하지 않는 prepare와 addPatty 를 구현했습니다.

```javascript
class Gunderia extends HambergerMaker {
    constructor() {
    }

    prepare() {
        console.log('재료 준비');
    }

    addPatty() {
        console.log('patty 추가');
    }
}
```

### 사용해 보기
위 구현을 통해 군데리아를 만들 수 있습니다.  
테스트를 해보기 위해 부모 클래스인 햄버거메이커를 먼저 돌려봤습니다.  
햄버거메이커에는 prepare와 addPatty 구현체가 없으므로 동작되지 않습니다. (throw 익셉션만 던지게 되어 있음)

```javascript

let hamberger = new HambergerMaker();

hamberger.create(); // 에러 발생. 서브 클래스에 주요 메서드가 구현되어 있지 않음~~

// 군데리아에서 상속받은 클래스를 이용해 생성
// 군데리아 클래스에는 prepare와 addPatty 가 구현되어 있으므로 문제 없음
let myGunderia = new Gunderia();
myGunderia.create();

```


## Template Method 사용의 장점과 주의점

1. 로직을 공통화 할 수 있다.
2. 상위 클래스와 하위 클래스의 연계한다.
3. 하위 클래스를 상위 클래스와 동일시한다.
4. 상속을 통하여 구현이 되므로 해당 추상 메소드가 필요한 클래스마다 상속을 받아야 한다.

## 왜 사용할까요?

템플릿 메소드 패턴은 알고리즘의 프레임을 맞추게 할 수 있습니다.   
정의된 알고리즘과 순서를 강제하지만 상속받은 클래스로 하여금 (오버라이드) 확장이 가능하게 해주는 디자인 패턴입니다.  
참고로 js 에는 추상 메소드와 override가 없기 때문에 명시적으로 해줄 수 없지만 ES6에서는 상속을 쓸 수 있으므로 유사하게 사용은 가능합니다.

