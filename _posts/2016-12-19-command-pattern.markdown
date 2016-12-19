---
layout: post
title:  "커맨드 패턴 (command pattern)"
date:   2016-12-19 22:12:00 +0900
categories: design pattern
---

커맨드패턴을 이용하면 요구사항을 객체로 캡슐화 할수 있으며, 매개변수를 써서 여러가지 다른 요구사항을 집어넣을 수도 있습니다. 
또한 요청내역을 큐에 저장하거나 로그로 기록할 수도 있으며, 작업취소 기능도 지원 가능합니다.
   

## 커맨드 패턴의 특징

1. 일련의 행동을 특정 리시버하고 연결을 통해 캡슐화한다.
2. execute()라는 메소드 하나만 외부에 공개한다.

커맨드 패턴에는 명령(command), 수신자(receiver), 발동자(invoker), 클라이언트(client)의 네개의 용어가 항상 따릅니다.  
커맨드 객체는 수신자 객체를 가지고 있으며, 수신자의 메서드를 호출하고, 이에 수신자는 자신에게 정의된 메서드를 수행합니다.
  
> 커맨드 객체는 별도로 발동자 객체에 전달되어 명령을 발동하게 합니다.  
> 발동자 객체는 필요에 따라 명령 발동에 대한 기록을 남길 수 있습니다.  
> 한 발동자 객체에 다수의 커맨드 객체가 전달될 수 있습니다.  
> 클라이언트 객체는 발동자 객체와 하나 이상의 커맨드 객체를 보유합니다.  
> 클라이언트 객체는 어느 시점에서 어떤 명령을 수행할지를 결정합니다.  
> 명령을 수행하려면, 클라이언트 객체는 발동자 객체로 커맨드 객체를 전달합니다.    



## 커맨드 패턴의 구조
커맨드는 가상의 데이터 타입인 커맨드 클래스(정확히는 인터페이스) 추가를 통해서 가능합니다.  
전체적인 구조는 커맨드와 이 커맨드 클래스를 확장하는 콘크리트 커맨드(Concrete Command), 커맨드 객체를 전달하는 인보커(invoker), 커맨드 객체를 생성하여 일련의 동작을 요청할 클라이언트(client), 동작을 수행하는 리시버(Receiver)로 구성됩니다.

 - 클라이언트 : 커맨드 객체를 생성하고 인보커를 통해 리시버에 전달하여 요청한다.
 - 인보커 : 클라이언트의 커맨드 객체를 리시버에 전달한다.
 - 리시버 : 요청대로 특정 행동을 수행한다.
 - 커맨드 : 어떠한 특정 행동을 담고 있는 추상 객체이다.

![factory screenshot]({{ site.url }}/images/attachment/comm-pattern.jpg)


## 커맨드 패턴의 예시

1. 손님이 웨이터에게 주문을 한다.
2. 웨이터가 고객의 주문을 주문서에 적는다.
3. 웨이터는 주문서를 주방에 전달하여 주문을 요청한다.
4. 요리사는 주문서에 적힌 주문대로 음식을 자신의 노하우로 만든다.

- 손님 == 클라이언트
- 웨이터 == Invoker 객체
- 주문서 == Command 객체
- 주방장 == Receiver 객체
- 주문을 하는것 == setCommand()
- 주문을 요청하는것 == execute()

## 커맨드 패턴의 구현

먼저 excute 메서드만 존재하는 간단한 인터페이스를 만듭니다.

```java
public interface Command {
	public void execute();
}

```


### 커맨드 객체 사용하기

```java
public class LightOnCommand implements Command{
	Light light;
	
	public LightOnCommand(Light light){
		this.light = light;
	}
	
	public void execute(){
		light.on();
	}
}
```

### 리모컨을 사용하기 위한 간단한 테스트 클래스

```java
public class SimpleRemoteControl{
	Command slot;
	
	public SimpleRemoteControl(){}

	public void setCommand(Command command){
		slot = command;
	}

	public void buttonWasPressed(){
		slot.execute();
	}
}
```

### 리모컨의 사용 예시

```java
public class RemoteControlTest{
	public static void main(String[] agrs){
		SimpleRemoteControlremote remote = new SimpleRemoteControl();
		Light light = new Light();
		LightOnCommand lightOn = new LightOnCommand(light);

		remote.setCommand(lightOn);
		remote.butonWasPressed();
	}
}
```

## 장단점
- 장점 : 요청실행부(receiver)와 호출하는 부분(Command 구현부)만 추가하면 동적으로 객체 호출이 가능하다
- 단점 : 객체 구성 부분이 추가 되면 추상부분부터 수정해서 추가해 줘야 한다.


참고 : Head First Design Patterns, 한빛미디어(주).