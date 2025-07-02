---
layout: post
title: 헤드퍼스트 디자인패턴 정리 12 - 복합 패턴
category: devlog
tags: [study, design-pattern]
image: /assets/img/blog/study/CompoundPattern.png
description: >
 헤드퍼스트 디자인패턴 책을 읽으면서 주요 내용을 정리해놓은 노트를 공개합니다.
comments: true
---

헤드퍼스트 디자인패턴 12장 복합 패턴에 대해 요약정리한 내용입니다. 

여러 패턴을 섞어서 쓰기.
여러 패턴을 함께  사용해서 다양한 디자인 문제를 해결하는 방법.
패턴 몇 개를 그냥 결합한다고 해서 복합패턴이라고 부르진 않고, 여러가지 문제의 일반적인 해결법을 제시해야한다. (ex.MVC패턴)

## 오리 문제를 돌아보자
#### 다른 울음소리를 내는 거위 등이 추가된다면?
- 어댑터 패턴을 써서 한 번 둘러싸주고 quack() 메소드를 호출할때 honk()가 자동으로 실행되도록 해줍니다.

```cs
public class GooseAdapter : Quackable
{
  Goose goose;
  public gooseAdapter(Goose goose)
  {
    this.goose = goose;
  }

  public void quack()
  {
    goose.honk();
  }
}


public class DuckSimulator
{
  void simulate()
  {
    Quackable mallardDuck = new MallardDuck();
    Quackable redheadDuck = new redheadDuck();
    // (...)
    // Goose를 Adapter로 감싸면 오리의 탈을 쓴 거위를 만들 수 있음
    Quackable gooseDuck = new GooseAdapter(new Goose());

	simulate(mallardDuck);
	simulate(redheadDuck);
	// (...)
	// Goose를 감싸고 나면 그냥 Quackable 오리들과 같은 방식으로 다룰 수 있음
    simulate(gooseDuck);
  }

  void simulate(Quackable duck)
  {
    duck.quack();
  }
}
```

#### 진짜 오리 소리가 난 횟수만 세고 싶다면?
- **데코레이터 패턴을 적용하여** quack()이 호출된 회수만 세게 만든다.
- quack() 메소드 호출 자체는 그 데코레이터로 싸여있는 Quackable 객체가 처리한다

```cs
public class QuackCounter :  Quackable
{
  Quckable duck;
  static int numberOfQuacks;
  public QuackCounter(Quackable duck)
  {
    this.duck = duck;
  }

  // 꽥꽥 소리를 내면서 동시에 꽥꽥거린 횟수를 갱신합니다.
  public void quack()
  {
    duck.quack();
    numberOfQuacks++;
  }

  public  static int getQuacks()
  {
    return numberOfQuacks;
  }
}
```

- **팩토리 패턴을 적용하여** 오리 객체를 생성하면서 데코레이터로 감싸는 부분을 따로 빼내어 캡슐화 한다

```cs
// 오리를 생산하기: 팩토리 패턴 적용
public abstrack class AbstractDuckFactory
{
  public abstract Quackable createMallardDuck();
  public abstract Quackable createRedheadDuck();
  // (...)
}

public class  DuckFactory : AbstractDuckFactory
{
  public Quackable createMallardDuck()
  {
    return new MallardDuck();
  }
  public Quackable createRedheadDuck()
  {
    return new RedheadDuck();
  }
  // (...)
}

// 진짜 사용할 클래스
// 모든 메소드에서 꽥꽥 소리를 낸 횟수를 세는 데코레이터로 감싼다
public class CountingDuckFactory : AbstractDuckFactory
{
  public Quackable createMallardDuck()
  {
    return new QuackConter(new MallardDuck());
  }
  public Quackable createRedheadDuck()
  {
    return new QuackConter(new RedheadDuck());
  }
}
```

- 시뮬레이터도  팩토리를 사용하도록 고친다

```cs
public class DuckSimulator
{
  public DuckSimulator()
  {
    AbstrackDuckFacory duckFactory = new CountingDuckFactory()
    simulate(duckFactory);
  }

  void simulate(duckFactory)
  {
    // 객체의 인스턴스를 직접 생성하지 않고, abstractDuckFactory 메소드 사용
    Quackable mallardDuck = duckFactory.createMallardDuck();
    Quackable redheadDuck = duckFactory.createredheadDuck();
    // (...)
    Quackable gooseDuck = new GooseAdapter(new Goose());

	simulate(mallardDuck);
	simulate(redheadDuck);
	// (...
    simulate(gooseDuck);
  }

  void simulate(Quackable duck)
  {
    duck.quack();
  }
} 
```

#### 오리 종 별로 관리하기
- 컴포지트 패턴을 사용하여 오리들을 모아 무리 단위로 관리
- 반복자 패턴도 사용하여 한번에 같은 작업 반복

```cs
// 무리 관리용 클래스
public class Flock : Quackable
{
  List<Quackable> quackers = new();

  public void add(Quackable quacker)
  {
    quackers.add(quacker);
  }
  
  public void quack()
  {
    // 반복자 패턴
    Iterator<Quackable> iterator = quackers.iterator();
    while(iterator.hasNext())
    {
      quacker.quack();
    }
  }
}

public class DuckSimulator
{
  // (...) main 메소드 생략

  public simulate(AbstractDuckFactory duckFactory)
  {
    // 이전코드와 마찬가지로 Quackable 생성
    Quackable mallardDuck = duckFactory.createMallardDuck();
    Quackable redheadDuck = duckFactory.createredheadDuck();
    // (...)
    Quackable gooseDuckOne = new GooseAdapter(new Goose());
    Quackable gooseDuckTwo = new GooseAdapter(new Goose());
    Quackable gooseDuckThree = new GooseAdapter(new Goose());

    // 무리 생성
    Flock flockofDucks = new Flock();
    flockOfDucks.add(mallardDuck);
    flockOfDucks.add(redheadDuck);
    // (...)

    // 다른 무리 생성
    Flock flockOfGoose = new Flock();
	flockOfGoose.add(gooseDuckOne);
	flockOfGoose.add(gooseDuckTwo);
	flockOfGoose.add(gooseDuckThree);

	// 새로 생긴 무리를 기존 무리에 추가
	flockOfDucks.add(flockOfGoose);

	// 모든 무리 테스트
	simulate(flockOfDucks);
	// 거위무리만 테스트
	simulate(flockOfGoose);
  }

  // 기존과 동일
  public simultate(Quackable duck)
  {
    duck.quack();
  }
}
```


#### 소리를 냈을 때 바로 연락받기
- 옵저버 패턴을 이용하여 객체의 행동을 관측하기

```cs
public interface IObserver
{
  public void update(QuackableObservable duck);
}

public class Observer : IObserver
{
  public void update(QuackObservable duck)
  {
    print($"{duck}이 방금 소리냈다.");
  }
}

public interface QuackObservable
{
  // 옵저버를 등록하는 메소드
  public void registerObserver(Observer observer);
  // 옵저버들에게 연락을 돌리는 메소드
  public void notifyObservers(); 
}

// Quackable이 옵저버 인터페이스를 구현하도록 만들기
public interface Quackable : QuackObservable
{
  public void quack;
}
```

- 등록 및 연락용 코드를 Observable 클래스에 캡슐화 후, 구성으로 QuackObservable에 포함시키기

```cs
public class Observable : QuackObservable
{
  List<Observer> observers = new List();
  QuackObservable duck;
  public Observable(QuackObservable duck)
  {
    this.duck = duck;
  }

  // 옵저버 등록 코드
  public void registerObserver(Observer observer)
  {
    observers.add(observer);
  }

  // 연락을 돌리는 코드
  public  void notifyObservers()
  {
    Iterator iterator = observers.iterator();
    while(iterator.hasNext())
    {
      Observer observer = itertor.next();
      observer.update(duck);
    }
  }
}

public class MallardDuck : Quackalbe
{
  Observable observable;
  // 본인을 옵저버의 인자로 전달
  public MallardDuck()
  {
    observable = new Observable(this);
  }

  public void quack()
  {
    print("꽥꽥");
    notifyObservers(); // 옵저버에게 바로 알리기
  }

  // QuackObservable에서 정의한 메소드들 바로 보조 객체에게 넘긴다.
  public void registerObserver(Observer observer)
  {
    observable.registerObserver(observer);
  }
  public void notifyObservers()
  {
     observable.notifyObservers();
  }
}
```

##  MVC 모델-뷰-컨트롤러 패턴
예를 들어 음악 소프트웨어를 사용한다고 한다면, 
- 모델
  : MP3 파일을 관리하고 재생하는데 필요한 모든 상태, 데이터, 애플리케이션 로직 등이 들어있음. 
  모델에서 뷰에게 상태가 변경되었음을 알림. 뷰에게 인터페이스 제공.
- 뷰
  : 일반적으로 화면에 표시할 때 필요한 상태와 데이터는 모델에서 직접 가져옴.
  뷰 디스플레이 갱신되는 것을 사용자가 볼 수 있음. 
- 컨트롤러
  : 사용자가 인터페이스를 건드리면 그 행동이 컨트롤러에게 전달됨. 
  모델에게 음악 재생을 요청.

### 규칙
- 사용자는 뷰에만 접촉할 수 있다
- 컨트롤러가 모델에게 상태를 변경하라고 요청
- 컨트롤러가 뷰를 변경해달라고 요청할 수도 있음
  (예를들어 특정 버튼을 활성화/비활성화)
- 상태가 변경되면 모델이 뷰에게 그 사실을 알림
- 뷰가 모델에게 상태를 요청함

### MVC에 사용되는 패턴 알아보기
- 모델 - 옵저버 패턴
  : 상태가 변경되었을 때 그 모델과 연관된 객체들에게 연락.
- 뷰 - 컴포지트 패턴
  : 디스플레이는 여러 단계로 겹쳐있는 윈도우, 패널, 버튼 등으로 구성됨.
  컨트롤러가 뷰에게 화면을 갱신해달라고 요청하면 최상위 뷰 구성  요소에게만 화면을  갱신하라고 하면 됨
- 컨트롤러 - 전략 패턴
  : 뷰 객체를 여러 전략을 써서 설정. 사용자가 요청한 내역을 처리할 때 모델과 뷰를 분리해주는 역할.


### MVC와 어댑터 패턴
컨트롤러의 전략패턴을 살펴보다보면 MVC에서 종종 쓰이는 어댑터 패턴도 만나게 된다.
**어떤 모델을 기존 뷰, 컨트롤러와 함께 사용하고 싶다면** 
어댑터를 사용하여 모델을 기존 모델에 맞게 적용시킨다.

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

복합 패턴
- 2개 이상의 패턴을 결합해서 일반적으로 자주 등장하는 문제들의 해법을 제공합니다.