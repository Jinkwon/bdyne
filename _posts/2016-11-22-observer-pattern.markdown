---
layout: post
title:  "Observer Pattern"
date:   2016-11-22 02:19:11 +0900
categories: jekyll update
---

디자인 패턴의 정리를 위해 글 작성을 시작했습니다.  
기본적인 패턴이자 널리 사용되는 패턴중 하나인 옵저버 패턴을 정리 해 보았습니다.  

---

## 이것이 무엇인고?
옵저버 패턴은 아주 흔하면서도 정말 많이 사용되는 디자인 패턴입니다.  
트위터의 follow 시스템을 이해하고 있다면 아주 빠르게 이해가 가능합니다.  

> 내가 좋아하는 연예인이 있다고 해 보겠습니다.  
> 그 연예인의 트위터를 follow를 하게 되면 그 뒤부터 그 가수가 올리는 트위터 메세지를 받아볼 수 있습니다.  
> 나는 구독자가 되고, 연예인은 발행자가 됩니다.  
> 연예인의 상태가 바뀔때마다 글을 올리면 나는 그의 상태 변화를 관찰할 수 있게 됩니다.  
> 연예인에게서 스캔들이 났을 때 그 연예인에게 더이상 관심이 없게 되고 un-follow를 합니다.  
> 그러면 더이상 그 연예인의 소식이 내게 보이지 않게 됩니다. (구독 취소)   

객체의 세계에 이 개념을 가져오면 서로간의 의존성을 줄이면서도 (느슨한 결합) 유연하게 객체간에 연결 고리를 만들 수 있게 됩니다.  

정리해보면,  
옵저버 패턴은 상태 변화를 관찰하는 관찰자들(옵저버)의 목록을 객체에 등록하고, 객체의 상태 변화가 있을 때마다 객체가 직접 관찰자들에게 변화를 통지하도록 하는 디자인 패턴 입니다.   
Pub/Sub 모델로도 불리기도 합니다. 1:N 의 객체 의존성을 정의할 수 있게 해주는 아주 고마운 디자인 패턴입니다.    


## 구현하기
자바스크립트에서는 아래와 같은 EventEmitter를 통해 패턴을 구현합니다.

먼저 스타 클래스를 만들어 보겠습니다.  
스타는 팬을 등록할 수 있고 팬을 삭제할 수 있습니다.   
그리고 EventEmitter 클래스를 상속 받음으로써 옵저버 패턴에 필요한 on과 emit을 사용할 수 있습니다.  

```javascript
/**
 * Star Class
 */
class Star extends EventEmitter {
  constructor() {
      this.fanList = [];
  }
  
  addFan(fan){
      this.fanList.push(fan);
  }
  
  removeFan(fan){
		this.fanList = this.fanList.filter((person) => {
		    return fan === person;
		});
  }
  
  sing() {
     this.emit('sing');
  }
  
  fallInLoveWith(name) {
     this.emit('scandle');
  }
}
```

다음으로는 팬을 만들어 보겠습니다. 


```javascript
/**
 * Fan Class
 */
class Fan {
   constructor(star) {
   }
   shout() {
       console.log('I love you!');
   }
}
```

그럼 시나리오를 간단히 만들어 봅시다.  

```javascript
let rockStar = new Star();

let sam = new Fan();
let elena = new Fan();

// rockStar의 팬이 된 sam과 elena
rockStar.addFan(sam);
rockStar.addFan(elena);

// rockStar가 노래를 하면 팬은 shout를 합니다
rockStar.on('sing', () => {
   sam.shout();
   elena.shout();
});

// 스캔들이 터지고 팬이 떠나갑니다
rockStar.on('scandle', () => {
   rockStar.removeFan(sam);
   rockStar.removeFan(elena);
});

// rockStar가 노래를 합니다.
rockStar.sing();

// rockStar가 Suji 와 사랑에 빠졌습니다.
rockStar.fallInLoveWith('Suji');
```

### 옵저버 패턴의 문제
옵저버 패턴이 은탄환(Siver bullet)은 아닙니다.  
객체를 모두  옵저버 패턴으로 연결할 경우 순환 실행이 발생할 수 있으며 이 경우 프로그램이 무한 루프에 빠질 수 있습니다.  
이를 주의해서 적절히 잘 사용한다면 매우 좋은 디자인 패턴임에는 분명합니다.  

### 정리
이런 간단한 구조를 응용하면 여러가지 형태의 클래스의 관계를 느슨한 결합으로 엮어줄 수 있습니다.  
실제 실무에서도 많이 사용되는 패턴이며 중요한 패턴이므로 꼭 익숙해져야 겠습니다.  