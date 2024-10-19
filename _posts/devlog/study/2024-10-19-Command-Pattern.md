---
layout: post
title: 헤드퍼스트 디자인패턴 정리 06 - 커맨드 패턴
category: devlog
tags: [study, design-pattern]
image: /assets/img/blog/study/CommandPattern.png
description: >
 헤드퍼스트 디자인패턴 책을 읽으면서 주요 내용을 정리해놓은 노트를 공개합니다.
comments: true
---

헤드퍼스트 디자인패턴 6장 커맨드 패턴에 대해 요약정리한 내용입니다.

리모컨을 제작한다고 해보자.

협력업체가 제공한 클래스에는 기기별로 class가 나뉘어져있고, 함수로는 각각 on/off, 시간 설정 등이 있다. 공통적인 인터페이스는 없고, 앞으로도 이런 클래스들이 더 많이 추가될 수도 있다는 조건이라면 어떻게 설계하는 것이 좋을까?

커맨드 패턴을 이용하면 된다!
리모컨 버튼마다 커맨드 객체를 저장해두고, 사용자가  버튼을 눌렀을 때 커맨드 객체로 작업을 처리하면 된다. 이렇게 분리한다면 리모컨은 아무것도 몰라도 된다.

# 커맨드 패턴 소개
이번엔 식당이라고 해보자.
- CreateOrder(): 고객이 주문서에 원하는 메뉴를 적는다
- TakeOrder(): 종업원이 주문서를 받아서 카운터에 전달한다
- OrderUp(): 음식을 준비하도록 지시사항을 만든다
- Make(): 주방장이 지시사항대로 음식을 준비한다

이러면 주문서는 여기저기 전달될 수 있는 객체가 되고, 종업원은 요리를 하지 않고 주문서만 전달한다. 그리고 주방장도 종업원은 몰라도 된다.
즉, 종업원과 주방장이 주문서 덕분에 분리될 수 있다.

이와같이 리모컨에도 주문서같은 객체를 추가시키면 된다.

# 인보커 로딩
코드로 한다면 이런 식이 될 것
- 클라이언트 : 커맨드 객체 생성
- 커맨드객체: 행동과 리시버 객체 정보를 들고 있다. execute() 메소드 하나만 제공
	```cs
	// 행동을 캡슐화하고 리시버에 있는 특정 행동을 처리
	public void Execute()
	{
	  receiver.Action1();
	  receiver.Action2();
	}
	```
- 인보커객체
	- setCommand() 메소드를 가지고 있고, 이를 클라이언트가 호출
	  이 때 커맨드 객체를 넘겨준다. 그 커맨드 객체는 나중에 쓰이기 전까지 인보커 객체에 보관
	- 커맨드 객체의 execute() 메소드를 호출
- 리시버객체
	- 리시버에 있는 행동메소드(action1, action2)가 커맨드에 의해 호출된다

즉 아래와같은 순서로 진행된다
1. 클라이언트에서 커맨드 객체를 생성
2. 클라이언트가 인보커객체에 있는 setCommand()를 호출해서 인보커에 커맨드 객체를 저장
3. 나중에 클라이언트에서 인보커에게 그 명령을 실행하라고 요청

비유로 따지면
- 고객: 클라이언트 객체
- 종업원: 인보커 객체
- 주문서: 커맨드 객체
- 주방장: 리시버 객체
- orderUp()음식을 준비하도록 지시사항을 만든다: execute() 
- takeOrder()종업원이 주문서를 받아서 카운터에 전달 : setCommand() 

# 커맨드 객체 만들기

커맨드 객체는 모두 같은 인터페이스를 구현해야 한다.
```cs
public interface Command
{
  public void Execute();
}
```

그리고 이를 구현한 클래스는 이렇다
```cs
public class Light
{
  public void On() {...}
  public void Off() {...}
}

public class LightOnCommand : Command
{
  Light light;
  // 무슨 조명을 켤 것인지 정보 전달
  public LightOnCommand(Light light)
  {
    this.light = light;
  }

  public void Execute()
  {
    ligth.on();
  }
}
```


해당 클래스는 다음과 같이 사용한다
```cs
public class SimpleRemoteControl
{
  Command slot; // 1개의 기기를 제어가능
  public SimpleRemoteControl(){}

  // 명령 설정. 언제든 커맨드를 바꿀 수 있다
  public void SetCommand(Command command)
  {
    this.slot = command;
  }
  
  // 버튼을 누르면 커맨드의 실행메소드를 호출
  public void ButtonWasPressed()
  {
    slot.execute();
  }
}
```

클라이언트로 다음과 같이 테스트 할 수 있다
```cs
public class RemoteControlTest
{
  public static void main(string[] args)
  {
    SimpleRemoteControl remote = new(); // 인보커(요청을 전달받고 실행)
    Ligth light = new(); // 리시버(요청을 받아서 처리)
    LightOnCommand lightOn = new(light); // 커맨드 객체. 리시버 전달됨

    remote.SetCommand(lightOn); // 커맨드객체 인보커에게 전달
    remote.ButtonWasPressed(); // 커맨드객체에게 실행 요청
  }
}
```


# 커맨드 패턴 정의

>[!커맨드 패턴]
>요청 내용을 객체로  캡슐화해서 객체를 서로 다른 요청 내역에 따라 매개변수화 할 수 있다. 이러면 요청을 큐에 저장하거나 로그로 기록하거나 작업 취소 기능을 사용할 수 있게 된다.

행동과 리시버를 한 객체에 넣고, 외부에 execute()라는 메소드 하나만 공개.

다이어그램으로 살펴본다면...
![](/assets/img/blog/study/Pasted%20image%2020241016181320.png)

# 슬롯에 명령 할당하기
리모컨 버튼(=슬롯)에 명령을 할당한다. 즉, 리모컨이 인보커가 되는 것.
사용자가 버튼을 누르면 그 버튼에 맞는 커맨드 객체의 execute() 메소드가 호출된다.

리모컨 객체에 Command를 배열로 두고, SetCommand에서 특정 슬롯(index)에 어떤 커맨드를 넣을지 결정한다. 그리고 실행할 때도 index를 받아 실행한다

# Undo 기능

- Undo 메소드 만들기
```cs
public interface Command
{
  public void Execute();
  public void Undo(); // undo 메소드를 추가
}
```
그리고 Undo메소드에 Execute가 on이라면 off를, off라면 on을 넣는식으로 동작시킬 수 있다

- 이전 상태를 기억하기
```cs
public enum FanState
{
  High, Medium, Low, Off
}
public FanState prevFanState = FanState.High; // 변경시 갱신
```

# 커맨드 패턴 활용

## 큐를 활용하기
커맨드들을 작업 큐에 넣고 하나씩 제거하면서 실행시키면 된다. 
실행이 끝나면 새로운 커맨드 객체를 가져온다.

## 모든 행동 기록 후 재호출하기
Command 인터페이스에 Store()과 Load() 메소드를 추가해서 구현할 수 있다.
Execute() 후 Store()하고, 복구 시 Load()를 호출 후 Execute()하면 된다.


# C#에서 자주사용하는 커맨드패턴
C#에는 delegate가 있어서 더 간편하게 사용할 수 있다. 책에는 안나오지만 추가로 정리.
delegate는 형식 안전한 함수 포인터로 메서드를 변수처럼 다루면서, 나중에 그 메서드를 호출할 수 있는 기능을 제공한다. 어떤 메서드가 호출될지 유연하게 결정할 수 있다는 것이 장점.

아래와같이 사용하면 Command 인터페이스와 구현클래스

Command 인터페이스는 delegate로 대체되고
```cs
public delegate void Command();
```

Invoker 클래스에서 이런 식으로 사용된다
```cs
public class RemoteControl
{
    private Command _command; // delegate

    public void SetCommand(Command command)
    {
        _command = command;
    }

    public void PressButton()
    {
        _command();
    }
}

```

Receiver
```cs
public class Light
{
    public void TurnOn()
    {
        Console.WriteLine("The light is on");
    }

    public void TurnOff()
    {
        Console.WriteLine("The light is off");
    }
}

```

최종적으로 클라이언트가 다음과 같이 사용한다
```cs
class Program
{
    static void Main(string[] args)
    {
        // Receiver
        Light light = new Light();

        // Invoker
        RemoteControl remote = new RemoteControl();

        // 명령을 델리게이트로 설정
        remote.SetCommand(light.TurnOn);
        remote.PressButton();  // "The light is on" 출력

        remote.SetCommand(light.TurnOff);
        remote.PressButton();  // "The light is off" 출력
    }
}

```


차이점 비교

|              | Command 인터페이스와 ConcreteCommand | delegate                        |
| ------------ | ------------------------------ | ------------------------------- |
| 복잡한 명령 처리    | 명령을 객체로 캡슐화하여 복잡한 명령도 처리 가능    | 단순한 명령 처리에 적합, 복잡한 명령에는 적합하지 않음 |
| 확장성          | 새로운 명령을 추가하기 쉽고 명령별로 구조화 가능    | 명령이 많아질수록 관리가 어려워짐              |
| 간결함          | 클래스가 많이 생기고 코드가 복잡해질 수 있음      | 클래스 없이 메서드만 전달하므로 코드가 간결        |
| Undo/Redo 처리 | 상태를 쉽게 저장하고 실행 취소 기능을 구현 가능    | 상태 저장이나 Undo 기능 구현이 어려움         |
| 적합한 프로젝트     | 대규모, 복잡한 명령을 처리하는 시스템에 적합      | 간단한 명령을 처리하는 소규모 프로젝트에 적합       |

# 마무리

객체지향 원칙
- 바뀌는 부분은 캡슐화한다
- 상속보다는 구성을 활용한다
- 구현보다는 인터페이스에 맞춰서 프로그래밍한다
- 상호작용하는 객체 사이에서는 가능하면 느슨한 결합을 사용해야한다
- 클래스는 확장에 열려 있어야 하지만 변경에는 닫혀 있어야 한다(OCP)
- 추상화된 것에 의존하게 만들고 구상 클래스에 의존하지 않게 만든다

커맨드 패턴
- 요청 내용을 객체로 캡슐화해서 객체를 서로 다른 요청 내역에 따라 매개변수화 할 수 있다. 이러면 요청을 큐에 저장하거나 로그로 기록하거나 작업 취소 기능을 사용할 수 있게 된다.