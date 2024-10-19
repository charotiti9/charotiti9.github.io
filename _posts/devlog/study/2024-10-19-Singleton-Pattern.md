---
layout: post
title: 헤드퍼스트 디자인패턴 정리 05 - 싱글턴 패턴
category: devlog
tags: [study, design-pattern]
image: /assets/img/blog/study/SingletonPattern.png
description: >
 헤드퍼스트 디자인패턴 책을 읽으면서 주요 내용을 정리해놓은 노트를 공개합니다.
comments: true
---

헤드퍼스트 디자인패턴 5장 싱글턴 패턴에 대해 요약정리한 내용입니다.
- 인스턴스를 하나만 만들어야하는 클래스가 있을 때 사용한다
- 필요할 때만 객체를 만들 수 있다

## 고전적인 싱글턴 패턴

이 고전적인 싱글턴 패턴에는 문제가 있다.
인스턴스가 필요한 상황이 닥치기 전까지 아예 인스턴스를 생성하지 않는다.
=> 게으른 인스턴스 생성 (Lazy Instantiation)

```cs
public class Singlton
{
  private static Singleton uniqueInstance;
  // 기타 인스턴스 변수

  // 생성자를 private으로 설정했기 때문에
  // 이 클래스 내에서만 클래스의 인스턴스를 만들 수 있다
  private Singleton();

  // 클래스이 인스턴스를 만들어서 반환
  public static Singleton getInstance()
  {
    if (uniqueInstance == null)
    {
      uniqueInstance = new Singleton();
    }
    return uniqueInstnace;
  }

  // 기타 메소드...
}
```


## 싱글턴 패턴의 정의

> [!싱글턴 패턴]
>클래스 인스턴스를 하나만 만들고, 그 인스턴스로의 전역 접근을 제공합니다.

- 클래스에서 하나뿐인 인스턴스를 관리하도록 만든다
- 그 어떤 클래스에서도 자신의 인스턴스를 추가로 만들지 못하게 한다
- 인스턴스가 필요하다면 반드시 클래스 자신을 거치도록 만든다
- 어디서든 그 인스턴스에 접근할 수 있도록 전역 접근 지점을 제공한다
- 요청이 들어오면 하나뿐인 인스턴스를 건네줄 수 있어야 한다

##  멀티스레딩 문제
2개의 스레드에서 아래의 코드를 사용한다고 가정했을 때,
```cs
ChocolateBoiler boiler = ChocolateBoiler.getInstance();
boiler.fill();
boiler.boil();
boiler.drain();
```

두 스레드가 다른 보일러 객체를 사용하게 될 가능성은 없는가?
```cs
// 고전적인 싱글턴 코드
public static ChocolateBoiler
{
  getInstance()
  {
    if(uniqueInstance == null)
    {
      uniqueInstance = new ChocolateBoiler;
    }
    return uniqueInstance;
  }
}
```

이러면 1번 스레드와 2번 스레드에서 동시에 getInstance를 했을 때, 
혹은 1번 스레드에서  아직 객체를 만들지 않았는데 2번 스레드에서 접근했을 때,
둘 다 null이므로 객체를 2개만들 위험이 있다.

### 동기화로 해결해봅시다
synchronized 키워드를 추가해서 
한 스레드가 메소드 사용을 끝내기 전까지는 다른 스레드를 기다리게 만든다. 
```cs
public class Singleton
{
  private static Singleton uniqueInstance;

  private Singleton(){}
  
  // synchronized 키워드 사용
  pubilc static synchronized Singleton getInstance()
  {
    if(uniqueInstance == null)
    {
      uniqueInstance = new Singleton();
    }
    return uniqueInstance;
  } 
}
```

그렇지만,
동기화가 꼭 필요한 시점은 이 메소드가 시작되는 때 뿐이다.
메소드를 동기화하면 성능이 100배정도 저하된다

### 더 효율적으로 해결하기

방법1. 인스턴스가 필요할 때는 생성하지 말고 처음부터 만든다
```cs
public class Singleton
{
  // 처음에 생성해버리고
  private static Singleton uniqueInstance = new Singleton();

  private Singleton(){}
  
  // 재호출 시에는 생성하지 않는다
  pubilc static Singleton getInstance()
  {
    return uniqueInstance;
  } 
}
```

방법2. DCL(Double-Checked Locking)을 써서 getInstance()에서 동기화 되는 부분을 줄인다

volatile 키워드: 컴파일러는 해당 변수를 최적화에서 제외하여 항상 메모리에 접근하도록 한다.
즉, 레지스터에 로드된 값을 사용하지 않고 매번 메모리를 참조한다.
```java
public class Singleton
{
  // volatile 키워드 추가
  private volatile static Singleton uniqueInstance;

  private Singleton(){}
  
  pubilc static Singleton getInstance()
  {
	// 인스턴스가 있는지 확인하고 없으면 동기화된 블록으로 들어감
	if(uniqueInstance == null)
	{
	  // 필요한 부분만 synchronized 키워드 사용
	  synchronize (Singleton.class)
      {
        // 다시한번 변수가 null인지 확인필요
	    if(uniqueInstance == null)
	    {
          uniqueInstance = new Singleton();
        }
      }
    }
    return uniqueInstance;
  } 
}
```

## c#의 경우...
책에는 c#에서 자주 사용하는 패턴말고 자바용 패턴으로 사용하는 듯하여 따로 정리해본다
참고: [Implementing the Singleton Pattern in C# (csharpindepth.com)](https://csharpindepth.com/articles/singleton)
#### not thread-safe
위에서 말한 고전적인 방식의 싱글턴 패턴. 사용비권장
```cs
// Bad code! Do not use!
public sealed class Singleton
{
    private static Singleton instance = null;

    private Singleton()
    {
    }

    public static Singleton Instance
    {
        get
        {
            if (instance == null)
            {
                instance = new Singleton();
            }
            return instance;
        }
    }
}
```

#### simple thread-safety
위에서 말한 synchronized 키워드와 비슷. lock을 걸어서 처리한다
다만 느리다... 
```cs
public sealed class Singleton
{
    private static Singleton instance = null;
    private static readonly object padlock = new object();

    Singleton()
    {
    }

    public static Singleton Instance
    {
        get
        {
            // 여기서 락을 건다
            lock (padlock)
            {
                if (instance == null)
                {
                    instance = new Singleton();
                }
                return instance;
            }
        }
    }
}
```

#### not lazy thread-safe (Static Constructor)
lazy 하지 않음. 
```cs
public sealed class Singleton
{
    private static readonly Singleton instance = new Singleton();

	// static 생성자는 클래스의 인스턴스가 생성되거나 정적 멤버가 참조될 때만 실행
	// 앱 도메인당 한 번만 실행되도록 지정
    static Singleton()
    {
    }

    // 인스턴스화를 방지하는 비공개 생성자.
    private Singleton()
    {
    }

    public static Singleton Instance
    {
        get
        {
            return instance;
        }
    }
}
```

#### lazy generic thread-safe
.NET 4 이상에서 동작하는 `System.Lazy<T>` 활용
```cs
public sealed class Singleton
{
    private static readonly Lazy<Singleton> lazy =
        new Lazy<Singleton>(() => new Singleton());

    public static Singleton Instance { get { return lazy.Value; } }

    private Singleton()
    {
    }
}
```


보통은 `Static Constructor`와 `Lazy<T>`를 많이 사용한다
- `Lazy<T>`: 실제로 필요할 때까지 객체를 생성하지 않음
- `Static Constructor`: 해당 싱글턴이 반드시 필요한 경우 코드가 더 단순하고 초기화가 보장됨

즉, 특정 싱글턴 객체가 반드시 필요하지 않은 경우에는 Lazy 방식이 더 나은 선택이 될 수 있음


# 마무리
객체지향 원칙
- 바뀌는 부분은 캡슐화한다
- 상속보다는 구성을 활용한다
- 구현보다는 인터페이스에 맞춰서 프로그래밍한다
- 상호작용하는 객체 사이에서는 가능하면 느슨한 결합을 사용해야한다
- 클래스는 확장에 열려 있어야 하지만 변경에는 닫혀 있어야 한다(OCP)
- 추상화된 것에 의존하게 만들고 구상 클래스에 의존하지 않게 만든다

싱글턴 패턴
- 클래스 인스턴스를 하나만 만들고, 그 인스턴스로의 전역 접근을 제공합니다.