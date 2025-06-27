---
layout: post
title: 헤드퍼스트 디자인패턴 정리 11 - 프록시 패턴
category: devlog
tags: [study, design-pattern]
image: /assets/img/blog/study/ProxyPattern.png
description: >
 헤드퍼스트 디자인패턴 책을 읽으면서 주요 내용을 정리해놓은 노트를 공개합니다.
comments: true
---

헤드퍼스트 디자인패턴 11장 프록시 패턴에 대해 요약정리한 내용입니다. 

객체 접근 제어하기
프록시는 자신이 대변하는 객체와 그 객체에 접근하려는 클라이언트 사이에서 여러 가지 방식으로 작업을 제어한다.

## 모니터링 코드 만들기
뽑기 기계를 모니터링 하고 싶다는 요청이 들어왔다

```cs
public class GumballMonitor
{
  GumballMachine machine;

  public GumballMachine(GumballMachine macine)
  {
    this.machine = machine;
  }
  
  public void report()
  {
    print("뽑기 기계 위치:" + machine.getLocation());
    print("현재 재고:" + machine.getCount());
    print("현재 상태:" + machine.getState());
  }
}
```

하지만, 요구사항이 바뀌어 원격으로 모니터링 하고 싶다면...
원격 프록시를 사용하면 된다.

## 원격 프록시의 역할
원격 프록시는 원격 객체의 **로컬 대변자 역할**.

- 원격 객체
  : 다른 주소 공간에서 돌아가고 있는 객체
- 로컬 대변자
  : 어떤 메소드를 호출하면, 다른 원격 객체에게 그 메소드 호출을 전달해주는 객체


![](/assets/img/blog/study/mermaid3.png)


클라이언트 객체는 원격 객체의 메소드 호출을 하는 것처럼 행동함.
하지만  실제로는 로컬 힙에 들어있는 '프록시'객체의 메소드를 호출함.
네트워크 통신과 관련된  저수준 작업은 이 프록시 객체에서 처리해주는 것...

이런경우를 보편화 시키면 아래와 같다

![](/assets/img/blog/study/mermaid4.png)

- 클라이언트 보조 객체는 진짜 서비스인척 하고 있지만, 실제로는 객체의 프록시일 뿐.
- 서비스 보조 객체는 클라이언트 보조 객체로 부터 요청을 받고, 받은 요청을 해석하여 진짜 서비스에 있는 메소드를 호출.

## 프록시 패턴이란

>[! 프록시 패턴]
>특정 객체로의 접근을 제어하는 대리인(특정 객체를 대변하는 객체)을 제공합니다.

예를들어 원격 객체, 생성하기  힘든 객체, 보안이 중요한 객체 등
다른 객체로의 접근을 제어하는 대리인 객체가 필요할 때 사용.

이걸 코드로 한다면...
```cs

// 먼저 같은 인터페이스를 상속받도록 인터페이스를 만들고
// 동일한 메소드를 호출하기 위해 함수를 만들어둠
public interface ISubject
{
  void Request();
}

// 프록시 뒤에 있는 진짜 객체는 실제 요청을 수행
public RealSubjet : ISubject
{
  public void Request()
  {
    print("RealSubject: 요청수행")
  }
}

// 진짜 객체를 숨겨주는 프록시는 객체에게 요청이 온 경우
// 처리할 거 처리하고 요청 수행 함수를 호출
public Proxy : ISubject
{
  private RealSubject _realSubject;
  public Proxy(RealSubject realSubject)
  {
    this._realSubject = realSubject;
  }

  public void Request)_
  {
    if(this.CheckAccess()) // 접근 가능한지 체크
    {
      this._realSubject.Request();
      this.LogAccess();
    }
  }

  // (...)
}

// 클라이언트에서 요청을 호출
public class Client(ISubject subject)
{
  // (...)
  subject.Request();
  // (...)
}
	
public class Program
{
  static void Main(string[] args)
  {
    Client client = new Client();
    RealSubject realSubject = new Subject();

    Proxy proxy = new Proxy(realSubject);
    client.ClientCode(proxy);
  }
}
```

## 원격 프록시와 가상 프록시 비교
프록시 패턴은 매우 다양한 형태로 씅지미나 결국 기본 프록시 디자인을 따른다.

- 원격 프록시
  : 다른 주소에 들어있는 객체의 대리인에 해당하는 로컬 객체
  프록시의 메소드를 호출하면 결국 그 호출이 네트워크로 전달되어, 결국 원격 객체의 메소드가 호출되는 것.
- 가상 프록시
  : 생성하는 데 많은 비용이 드는 객체를 대신한다
  진짜 객체가 필요한 상황이 오기 전까지 객체의 생성을 미룬다. (ex. Image 로딩 등)
  혹은 객체 생성 전이나 도중에 객체를 대신하기도 한다.
  후에 객체 생성이 끝나면 RealSubject에게 요청을 전달

### 가상프록시 예제
이미지 프록시 코드를 만든다고 해보자

```cs

public interface IImage
{
  void Display();
}

public class RealImage : IImage 
{ 
	private string _fileName;
	public RealImage(string fileName) 
	{ 
		_fileName = fileName;
		LoadImageFromDisk(); 
	} 
	
	public void Display() 
	{ 
		Console.WriteLine("이미지 보여주기: " + _fileName); 
	} 
	private void LoadImageFromDisk() 
	{ 
		Console.WriteLine("이미지 로딩하기: " + _fileName); 
	} 
}


public class ImageProxy : IImage
{
	private RealImage _realImage;
	private string _fileName;
	public ImageProxy(string fileName)
	{
		_fileName = fileName;
	}

	public void Display()
	{
		if (_realImage == null) // 여기서 대기하는 코드를 추가시킬 수 있음
		{
			// 현재는 이미지가 없을 때 불러오는 일만 함
			_realImage = new RealImage(_fileName);
		}
		_realImage.Display();
	}
}

class Program
{
	static void Main(string[] args)
	{
		IImage image = new ImageProxy("test.jpg");
		image.Display();
	}   
}

```

## 다른 프록시들
- 캐싱 프록시
  : 기존에 생성했던 객체들을 캐시에 저장해 뒀다가, 요청이 들어왔을 때 캐시에 저장되어 있는 객체를 리턴한다.
- 보호 프록시
  : 접근 권한을 바탕으로 객체로의 접근을 제어하는 프록시.
- 방화벽 프록시
  : 일련의 네트워크 자원으로서의 접근을 제어함으로써 '나쁜' 클라이언트로부터 보호
- 스마트 레퍼런스 프록시
  : 주제가 참조될 때무다 추가 행동을 제공한다(ex. 객체의 레퍼런스 개수를 센다)
- 동기화 프록시
  : 여러 스레드에서 주제에 접근할 때 안전하게 작업을 처리할 수 있게 해준다
- 복잡도 숨김 프록시
  : 복잡한 클래스의 집합으로의 접근을 제어, 그  복잡도를 숨겨준다.
  퍼사드 프록시라고 부르기도 한다. 퍼사드 패턴과의 차이점은 프록시는 접근을 제어하지만, 퍼사드 패턴은 대체 인터페이스만 제공한다는 점에서 있다.
- 지연 복사 프록시
  : 클라이언트에서 필요로 할 때까지 객체가 복사되는 것을 지연시킴으로써 객체의 복사를 제어.
  변형된 가상 프록시라고 할 수 있다.

## 비교하기
- 데코레이터: 클래스에 새로운 행동을 추가하는 용도
- 프록시: 어떤 클래스로의 접근을 제어하는 용도

- 어댑터:  다른 객체의 인터페이스를 바꿔줌
- 프록시: 똑같은 인터페이스를 사용한다

- 데코레이터
  :  다른 객체를 감싸서 새로운 행동을 추가
- 퍼사드
  : 여러 객체를 감싸서 인터페이스를 단순화
- 어댑터
  : 다른 객체를 감싸서 다른 인터페이스를 제공
- 프록시
  : 다른 객체를 감싸서 접근을 제어



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

프록시 패턴
- 특정 객체로의 접근을 제어하는 대리인(특정 객체를 대변하는 객체)을 제공합니다.

