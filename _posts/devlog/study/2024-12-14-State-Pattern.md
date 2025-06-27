---
layout: post
title: 헤드퍼스트 디자인패턴 정리 10 - 상태 패턴
category: devlog
tags: [study, design-pattern]
image: /assets/img/blog/study/StatePattern.png
description: >
 헤드퍼스트 디자인패턴 책을 읽으면서 주요 내용을 정리해놓은 노트를 공개합니다.
comments: true
---

헤드퍼스트 디자인패턴 10장 상태 패턴에 대해 요약정리한 내용입니다. 

사실 상태패턴은 전략패턴과 쌍둥이다. 
전략패턴은 바꿔 쓸 수 있는 알고리즘을 내세웠고,
상태패턴은 내부 상태를 바꿈으로써 객체가 행동을 바꿀 수 있도록 도와준다.
꽤 다르지만 그 밑바탕에 깔린 설계는 거의 같다.

>[! 상태 패턴]
>상태패턴을 사용하면 객체의 내부 상태가 바뀜에 따라서 객체의 행동을 바꿀 수 있습니다. 마치 객체의 클래스가 바뀌는 것과 같은 결과를 얻을 수 있습니다.

책에서는 우리가 잘아는 enum/switch문이 아닌 state라는 인터페이스를 구현하는 것으로 상태패턴을 구현.

아래의 코드는 책과는 좀 다르지만... 훨씬 일반적이라 이렇게 넣었음.

State 인터페이스
```cs
public interface State
{
    void Enter();    // 상태에 진입할 때 호출되는 메서드
    void Execute();  // 현재 상태에서 지속적으로 호출되는 메서드
    //  그 외 다양한 특수 메서드도 추가될 수 있다
    void Exit();     // 상태를 종료할 때 호출되는 메서드
}
```

상태 클래스 구현
```cs
public class IdleState : State
{
    public void Enter()
    {
        Console.WriteLine("Entering Idle State.");
    }

    public void Execute()
    {
        Console.WriteLine("Idling...");
    }

    public void Exit()
    {
        Console.WriteLine("Exiting Idle State.");
    }
}

public class WalkingState : State
{
    public void Enter()
    {
        Console.WriteLine("Entering Walking State.");
    }

    public void Execute()
    {
        Console.WriteLine("Walking...");
    }

    public void Exit()
    {
        Console.WriteLine("Exiting Walking State.");
    }
}
```

State들을 사용하는 class
```cs
public class Character
{
    private State currentState;

    public void SetState(IState newState)
    {
        // 현재 상태 종료
        currentState?.Exit();

        // 새로운 상태로 전환
        currentState = newState;

        // 새로운 상태에 진입
        currentState.Enter();
    }

    public void Update()
    {
        // 현재 상태에서 동작 실행
        currentState?.Execute();
    }
}
```

테스트코드
```cs
class Program
{
    static void Main(string[] args)
    {
        Character character = new Character();

        // Idle 상태로 전환
        character.SetState(new IdleState());
        character.Update();

        // Walking 상태로 전환
        character.SetState(new WalkingState());
        character.Update();
    }
}
```

## 상태패턴과 전략패턴
둘의 용도는 다르다. 

- 상태 패턴
	- 객체의 내부 상태가 변함에 따라 자동으로 행동이 달라진다
	- Context 객체에서 여러 상태 객체 중 한 객체에게 모든 행동을 맡기게 되고 그 객체의 내부 상태에 따라 현재 상태를 나타내는 객체가 바뀐다
		- Context? 상황에 맞는 특정 행동을 수행하는 객체.
		- Context 객체는 미리 정해진 상태 전환 규칙에 따라서 알아서 자기 상태를 변경
		  즉, 원래 상황에 맞게 상태를 바꾸는 걸 염두에 둔 디자인
	- 클라이언트는 상태 객체를 몰라도 된다
	- 상태 관리를 강조
- 전략 패턴
	- 행동 방식을 분리하여 필요에 따라 클라이언트가 유연하게 선택할 수 있다
	- 클라이언트가 Context 객체에게 어떤 전략 객체를 사용할지 지정해줌
		- 어떤 클래스의 인스턴스를 만들고, 그 인스턴스에게 어떤 행동을 구현하는 전략객체를 건네줌.
		- 객체가 상태를 변경하는 것을 장려하지는 않음
	- 행동 방식 선택을 강조


### +추가
- 상태 패턴
  : 상태를 기반으로 하는 행동을 캡슐화하고, 행동을 현재 상태에게 위임
- 전략 패턴
  : 바꿔 쓸 수 있는 행동을 캡슐화한 다음, 실제 행동은 다른 객체에게 위임
- 템플릿 메소드 패턴
  : 알고리즘의 각 단계를 구현하는 방법을 서브클래스에서 구현


# 마무리

객체지향 원칙
- 바뀌는 부분은 캡슐화한다
- 상속보다는 구성을 활용한다
- 구현보다는 인터페이스에 맞춰서 프로그래밍한다
- 상호작용하는 객체 사이에서는 가능하면 느슨한 결합을 사용해야한다
- 클래스는 확장에 열려 있어야 하지만 변경에는 닫혀 있어야 한다(OCP)
- 추상화된 것에 의존하게 만들고 구상 클래스에 의존하지 않게 만든다
- 진짜 절친에게만 이야기해야한다 = 최소결합
  직접 알 필요가 없는 다른 객체와 상호작용하는 것을 피하라
- 저수준 구성요소가 시스템에 접속할 수는 있지만 언제 어떻게 그 구성 요소를 사용할지는 고수준 구성 요소가 결정한다 (순환의존 X)
- 어떤 클래스가 바뀌는 이유는 하나뿐이어야만 한다

상태 패턴
- 내부 상태가 바뀜에 따라 객체의 행동이 바뀔 수 있도록 해 줍니다.
- 마치 객체의 클래스가 바뀌는 것 같은 결과를 얻을 수 있습니다.