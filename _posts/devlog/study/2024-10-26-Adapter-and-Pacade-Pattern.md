---
layout: post
title: 헤드퍼스트 디자인패턴 정리 07 - 어댑터패턴과 퍼사드패턴
category: devlog
tags: [study, design-pattern]
image: /assets/img/blog/study/AdapterAndPacade.png
description: >
 헤드퍼스트 디자인패턴 책을 읽으면서 주요 내용을 정리해놓은 노트를 공개합니다.
comments: true
---

헤드퍼스트 디자인패턴 7장 어댑터패턴과 퍼사드 패턴에 대해 요약정리한 내용입니다. 

# 어댑터 패턴
어떤 인터페이스를 클라이언트에서 요구하는 형태로 적응 시키는 역할

새로운 업체에서 제공한 클래스 라이브러리를 사용해야하는데 해당 업체의 인터페이스가 기존에 사용하던 인터페이스와 다르다면?
중간에 기존시스템과 업체에서 제공한 클래스를 이어주고 변환시켜주는 중개인 역할의 어댑터가 필요하다.

```cs
public class TurkeyAdapter : Duck
{
  Turkey turkey;
  public TurkeyAdapter(Turkey turkey)
  {
    this.turkey = turkey;
  }

  // 칠면조는 꽥이 없어서 대신 넣어준 모습
  public void quak()
  {
    turkey.gobble();
  }

  public void fly()
  {
    for(int i = 0; i < 5; i++)
    {
      turkey.fly(); // 칠면조는 멀리 날지 못하기 때문에 변환해줌
    }
  } 
}
```

- 클라이언트에서 타겟 인터페이스로 메소드를 호출해서 어댑터에 요청을 보냄
- 어댑터는 어댑티 인터페이스(turkey)로 그 요청을 어댑티에 관한 메소드 호출로 변환
- 클라이언트는 호출 결과를 받긴 하지만 중간에 어댑터가 있다는 사실을 모름

## 어댑터 패턴이란
특정 클래스 인터페이스를 클라이언트에서 요구하는 다른 인터페이스로 변환합니다. 인터페이스가 호환되지 않아 같이 쓸 수 없었던 클래스를 사용할 수 있게 도와줍니다.

## 구조

1. Target: 클라이언트가 사용하려고 하는 인터페이스
2. Adaptee: `Target`과 다른 인터페이스를 가진 클래스
3. Adapter: `Target` 인터페이스를 구현하고 `Adaptee` 클래스를 감싸서 `Target`과 `Adaptee`가 호환되도록 해주는 클래스
4. Client: `Target` 인터페이스를 사용하여 `Adaptee`와 통신

### 클래스 어댑터
클래스 어댑터 패턴은 다중 상속을 필요로 한다
어댑터 클래스가 Target 인터페이스와 Adaptee 클래스를 동시에 상속
- 클라이언트는 오리에게 요청하고 있다고 생각한다
- 타겟은 Duck클래스
- Turkey클래스도 어댑터에서 Duck의 메소드 호출을 가로채서 변환한다
- 어댑터는 Duck과 Turkey를 모두 확장한 클래스

**특징**
- 어댑터 클래스가 `Target` 인터페이스와 `Adaptee` 클래스 모두를 상속하므로 두 클래스 간의 관계가 더 밀접하게 연결된다
- 다중 상속이 가능한 언어에서만 가능하며, C#에서는 다중 상속이 불가능하기 때문에 많이 사용되지 않는다. (인터페이스로 구현은 가능)

```cs
// Target 인터페이스 정의
public interface ITarget
{
    void Request();
}

// Adaptee 클래스 정의(호환되지 않는 인터페이스)
public class Adaptee
{
    public void SpecificRequest()
    {
        Console.WriteLine("Adaptee의 SpecificRequest 호출됨");
    }
}

// 클래스 어댑터 (Target 인터페이스 구현 + Adaptee 클래스 상속)
public class ClassAdapter : Adaptee, ITarget
{
    public void Request()
    {
        // Adaptee의 SpecificRequest 메서드를 호출
        SpecificRequest();
    }
}

// 사용 예시
class Program
{
    static void Main()
    {
        ITarget adapter = new ClassAdapter();
        adapter.Request(); // Adaptee의 SpecificRequest가 호출됨
    }
}

```


### 객체 어댑터
구성(composition)하여 사용
어댑터 클래스는 `Target` 인터페이스를 구현하지만 `Adaptee`의 인스턴스를 포함하여 이를 통해 필요한 메서드를 호출
- 클라이언트는 오리에게 요청하고 있다고 생각한다
- 타겟은 Duck인터페이스
- 어댑터는 Duck인터페이스만 구현한다
- Turkey객체는 어댑터 덕분에 호출될 수 있다

**특징**
- 다중 상속이 필요 없기 때문에 C#과 같은 언어에서도 사용하기 적합
- 어댑터가 `Adaptee` 인스턴스를 포함하여 상속보다는 구성(composition)을 사용하기 때문에 더 유연함
- Adaptee의 서브클래스에서도 쉽게 확장하여 사용할 수 있음

```cs
// Target 인터페이스 정의
public interface ITarget
{
    void Request();
}

// Adaptee 클래스 정의 (호환되지 않는 인터페이스)
public class Adaptee
{
    public void SpecificRequest()
    {
        Console.WriteLine("Adaptee의 SpecificRequest 호출됨");
    }
}

// 객체 어댑터 (Target 인터페이스 구현 + Adaptee 구성)
public class ObjectAdapter : ITarget
{
    private readonly Adaptee _adaptee;

    public ObjectAdapter(Adaptee adaptee)
    {
        _adaptee = adaptee;
    }

    public void Request()
    {
        // Adaptee의 SpecificRequest 메서드를 호출
        _adaptee.SpecificRequest();
    }
}

// 사용 예시
class Program
{
    static void Main()
    {
        Adaptee adaptee = new Adaptee();
        ITarget adapter = new ObjectAdapter(adaptee);
        adapter.Request(); // Adaptee의 SpecificRequest가 호출됨
    }
}

```

### 둘의 차이
- 객체 어댑터
	- 구성을 사용
	- 어댑티 클래스와 그 서브클래스에 대해서 어댑터 역할이 가능
- 클래스 어댑터
	- 특정 어댑티 클래스에만 적용할 수 있음
	- 대신 어댑티 전체를 다시 구현하지 않아도 된다
	- 그리고 서브 클래스라 어댑티의 행동을 오버라이드 가ㅏ능

-> 클래스를 직접 상속받아야 하는 경우에는 클래스 어댑터가 유용할 수 있지만, 
객체 어댑터가 대체로 더 많이 활용된다

# 퍼사드 패턴
인터페이스를 단순하게 바꾸려고 인터페이스를 변경할 때 사용
복잡한 시스템을 편리하게 사용할 수 있도록 해준다

- 퍼사드 클래스를 하나 만든다
- 퍼사드 클래스는 각기 다른 클래스들을 구성요소를 하나의 서브시스템으로 간주
  (예를 들어 watchMovie()메소드는 서브시스템들의 메소드를 호출하여 필요한 작업을 처리한다. turnOn(), setStreamingPlayer()...)
- 클라이언트 코드는 서브시스템이 아닌 퍼사드 클래스의 메소드를 호출
- 여전히 클라이언트는 고급동작이 필요할 때 서브시스템에 접근할 수 있다

## 퍼사드패턴 정의
서브시스템에 있는 일련의 인터페이스를 통합 인터페이스로 묶어 줍니다. 또한 고수준 인터페이스도 정의하므로 서브시스템을 더 편리하게 사용할 수 있습니다.

## 퍼사드 패턴의 구조
1. Facade(퍼사드): 서브 시스템의 복잡한 기능을 단순화한 인터페이스를 제공하여, 클라이언트가 이를 통해 서브 시스템에 접근할 수 있게 해줌
2. Subsystem Classes(서브 시스템 클래스들): 퍼사드가 단순화한 인터페이스 뒤에 감춰진 실제 기능을 제공하는 클래스들입니다. 클라이언트가 서브 시스템에 직접 접근하지 않고 퍼사드를 통해 접근
3. Client(클라이언트): 서브 시스템의 복잡성을 신경 쓰지 않고, 퍼사드를 통해 시스템의 기능을 이용
   
## 예시
서브시스템 클래스들
```cs
// 서브 시스템 1: 스크린
public class Screen
{
    public void Lower() => Console.WriteLine("스크린을 내립니다.");
    public void Raise() => Console.WriteLine("스크린을 올립니다.");
}

// 서브 시스템 2: 프로젝터
public class Projector
{
    public void TurnOn() => Console.WriteLine("프로젝터를 켭니다.");
    public void TurnOff() => Console.WriteLine("프로젝터를 끕니다.");
}

// 서브 시스템 3: 사운드 시스템
public class SoundSystem
{
    public void TurnOn() => Console.WriteLine("사운드 시스템을 켭니다.");
    public void TurnOff() => Console.WriteLine("사운드 시스템을 끕니다.");
    public void SetVolume(int level) => Console.WriteLine($"사운드 볼륨을 {level}로 설정합니다.");
}
```

퍼사드 클래스스
```cs
// Facade 클래스: HomeTheaterFacade
public class HomeTheaterFacade
{
    private readonly Screen _screen;
    private readonly Projector _projector;
    private readonly SoundSystem _soundSystem;

    public HomeTheaterFacade(Screen screen, Projector projector, SoundSystem soundSystem)
    {
        _screen = screen;
        _projector = projector;
        _soundSystem = soundSystem;
    }

    public void StartMovie()
    {
        Console.WriteLine("영화를 시작합니다...");
        _screen.Lower();
        _projector.TurnOn();
        _soundSystem.TurnOn();
        _soundSystem.SetVolume(10);
    }

    public void EndMovie()
    {
        Console.WriteLine("영화를 종료합니다...");
        _screen.Raise();
        _projector.TurnOff();
        _soundSystem.TurnOff();
    }
}
```

클라이언트 코드
```cs
// 클라이언트 코드
class Program
{
    static void Main()
    {
        // 서브 시스템 인스턴스 생성
        var screen = new Screen();
        var projector = new Projector();
        var soundSystem = new SoundSystem();

        // Facade 생성
        var homeTheater = new HomeTheaterFacade(screen, projector, soundSystem);

        // 영화를 시작하고 종료하는 단순화된 인터페이스 사용
        homeTheater.StartMovie();  // 복잡한 서브 시스템 호출을 단순화함
        homeTheater.EndMovie();    // 복잡한 서브 시스템 호출을 단순화함
    }
}
```

## 장점
- 간단한 사용법 제공
  : 클라이언트는 복잡한 내부 구조를 알 필요 없이, 퍼사드의 간단한 메서드만 호출하면 된다
- 서브 시스템의 독립성 유지
  : 클라이언트는 퍼사드만 알고 서브 시스템 클래스들과 직접 상호작용하지 않으므로, 서브 시스템 변경 시 클라이언트에 영향을 미치지 않는다

퍼사드 패턴은 특히 외부에서 접근할 때 단순화된 인터페이스가 필요한 복잡한 시스템에서 유용



# 최소 지식 원칙
직접 알 필요가 없는 다른 객체와 상호작용하는 것을 피하라.

객체 간의 의존성을 최소화하여 시스템의 결합도를 낮추는 설계 원칙
객체가 직접적으로 알 필요가 없는 다른 객체와의 상호작용을 피한다.

다른말로
말하는 사람의 원칙(Law of Demeter), 혹은 데메테르 법칙이라고도 한다.

객체 사이의 상호작용은 될 수 있으면 아주 가까운 '친구' 사이에서만 허용
-> 어떤 객체든 그 객체와 상호작용을 하는 클래스의 객체와 상호작용 방식에 주의를 기울여야한다는 뜻

어떻게 하면 최소한의 객체와 상호작용할 수 있을까

친구를 만들지 않는 4개의 가이드라인
- 객체 자체
```cs
public class Customer
{
    private decimal balance;

    public void PrintBalance()
    {
        // 자기 자신에게 접근 (허용됨)
        Console.WriteLine("잔액: " + balance); 
    }
}

```

- 메소드에 매개변수로 전달된 객체
```cs
public class Bank
{
    public void PrintAccountBalance(BankAccount account)
    {
        // 매개변수로 전달된 객체에 접근 (허용됨)
        Console.WriteLine("잔액: " + account.GetBalance());
    }
}
```

- 메소드를 생성하거나 인스턴스를 만든 객체
```cs
public class Customer
{
    public void CreateAccount()
    {
	    // 메서드 내부에서 생성한 객체
        BankAccount account = new BankAccount(100);
        Console.WriteLine("잔액: " + account.GetBalance()); // 접근 가능 (허용됨)
    }
}

```

- 객체에 속하는 구성 요소
```cs
public class Customer
{
    public BankAccount Account { get; set; } // Customer의 구성 요소

    public void PrintAccountBalance()
    {
	    // 구성 요소에 접근 (허용됨)
        Console.WriteLine("잔액: " + Account.GetBalance()); 
    }
}

```


## 원칙을 따르지 않는 경우
예시 1
```cs
public float getTemp()
{
  // 중간 객체 station의 getTemperature() 호출
  Thermometer thermometer = station.getTemperature();
  // 또 다른 객체 thermometer의 getTemperature() 호출
  return thermometer.getTemperature();
}
```
- `station` 객체의 `getTemperature()` 메서드를 통해 `Thermometer` 객체가 반환. 
  즉, **객체가 자신과 직접적으로 연관이 없는 객체** (`Thermometer`)에 접근함
- `getTemp()` 메서드는 `station` 객체에만 의존하는 것이 아니라, 
  `station`이 반환하는 `Thermometer` 객체의 내부 동작에도 의존. 
  따라서 결합도가 높아지고, **`Thermometer` 클래스나 `getTemperature()` 메서드가 변경되면 이 코드를 수정해야 할 가능성**이 높아집니다.


예시2
```cs
// Customer가 BankAccount의 Balance 속성에 직접 접근
public class Customer
{
    public BankAccount Account { get; set; }

    public void PrintAccountBalance()
    {
        Console.WriteLine("잔액: " + Account.Balance);
    }
}

public class BankAccount
{
    public decimal Balance { get; private set; }

    public BankAccount(decimal initialBalance)
    {
        Balance = initialBalance;
    }
}
```
## 원칙을 따르는 경우 (수정한 코드)

예시 1
```cs
// Station 클래스에 온도를 반환하는 메서드 추가
public class Station
{
    private Thermometer thermometer;

    public float getTemperature()
    {
	    // Thermometer 객체의 세부 사항은 Station이 처리
        return thermometer.getTemperature();
    }
}

// 수정된 getTemp() 메서드
public float getTemp()
{
    return station.getTemperature(); // station에게 메시지를 보내어 온도를 얻음
}
```
- 중간 객체인 `station`이 필요한 정보를 제공하는 메서드를 추가
- `Thermometer` 객체에 직접 접근하지 않고, `station`을 통해 간접적으로 온도를 얻는다


예시2.
```cs
public class Customer
{
    public BankAccount Account { get; set; }

    public void PrintAccountBalance()
    {
        // Account에게 메시지를 보내어 Balance 값을 얻음
        Console.WriteLine("잔액: " + Account.GetBalance());
    }
}

public class BankAccount
{
    private decimal Balance;

    public BankAccount(decimal initialBalance)
    {
        Balance = initialBalance;
    }

	// 
    public decimal GetBalance()
    {
        return Balance;
    }
}
```


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

어댑터 패턴
- 특정 클래스 인터페이스를 클라이언트에서 요구하는 다른 인터페이스로 변환한다. 인터페이스가 호환되지 않아 같이 쓸 수 없었던 클래스르 사용할 수 있게 도와준다

퍼사드 패턴
- 서브시스템에 있는 일련의 인터페이스를 통합 인터페이스로 묶어준다. 또한 고수준 인터페이스도 정의하므로 서브시스템을 더 편리하게 사용할 수 있다.