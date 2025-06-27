---
layout: post
title: 헤드퍼스트 디자인패턴 정리 09 - 반복자 패턴과 컴포지트 패턴
category: devlog
tags: [study, design-pattern]
image: /assets/img/blog/study/IteratorAndComposite.png
description: >
 헤드퍼스트 디자인패턴 책을 읽으면서 주요 내용을 정리해놓은 노트를 공개합니다.
comments: true
---

헤드퍼스트 디자인패턴 9장 반복자 패턴과 컴포지트 패턴에 대해 요약정리한 내용입니다. 

컬렉션을 잘 관리하기
컬렉션에 들어있는 모든 객체에게 일일히 접근하고 싶다면?
객체 저장방식을 보여주지 않으면서 객체에 일일히 접근할 수 있게 해주는 방법을 알아보자.

# 반복자 패턴
만약 배열에 저장된 아이템들과 리스트에 저장된 아이템들을 모두 함께 순환하려면 어떻게 하는것이 좋을까?
그 때 **반복을 캡슐화**하면 된다.

- Iterator 인터페이스
	- hasNext()
	- next()
- 배열 Iterator 및 리스트 Iterator
	- 위의 인터페이스를 구현
- 그리고 배열과 리스트를 동시에 순환하려는 클라이언트
	- Iterator로 받아서 메소드 사용

>[! 반복자 패턴]
>컬렉션의 구현 방법을 노출하지 않으면서 집합체 내의 모든 항목에 접근하는 방법을 제공합니다.

구조는 다음과 같다
- Client
- Aggregate 인터페이스
	- createIterator() 메소드
	- 공통된 인터페이스로 클라이언트가 부르기 편리하도록 함
- ConcreteAggregate: Aggregae인터페이스
	- Iterator로 컬렉션 리턴
- Iterator 인터페이스
	- hasNext, next, remove() 메소드
	- 보통 언어에서 기본으로 제공하는 것을 사용
- ConcreteIterator
	- Iterator인터페이스 구현
	- ConcreteAggregate로인해 생성됨
	- 반복 작업 중 현재 위치를 관리하는 역할

- 해시테이블같이 정해진 순서가 없는 컬렉션의 반복 작업 순서는 어떻게 정하는가?
  : 반복자에는 특별한 순서가 정해져있지 않음. 접근 순서는 사용된 컬렉션의 특성 및 구현과 연관되어있고, 일반적으로 특별하게 언급이 되어 있지 않은 이상 순서를 가정하면 안된다.

# 단일 역할 원칙

>[! 디자인 원칙]
>어떤 클래스가 바뀌는 이유는 하나뿐이어야 한다.
>하나의 클래스는 하나의 역할만 한다

- 컬렉션이 어떤 이유로 바뀌게 되면 그 클래스도 바뀌어야한다
- 반복자 관련 기능이 바뀌었을 때도 클래스가 바뀌어야한다.

클래스를 고치는 일은 최대한 피해야한다. 변경할만한 이유가 2가지가 되면 그만큼 고쳐야할 가능성이 커지고 디자인에 있어서 2가지 부분에 영향이 미친다는 뜻이다.

종종 2가지 이상의 역할을 가지는 행동을 하나로 묶어서 인식하곤 하는데 이를 조심해야한다.

# 컴포지트 패턴
반복자 패턴만으로 처리하기 어려운 부분이 있다면

>[! 컨포지트 패턴]
>객체를 트리구조로 구성해서 부분-전체 계층구조를 구현합니다. 컴포지트 패턴을 사용하면 클라이언트에서 개별 객체와 복합 객체를 똑같은 방법으로 다룰 수 있습니다.

- 부분-전체 계층구조
  : 부분들이 계층을 이루고 있지만 모든 부분을 묶어서 전체로 다룰 수 있는 구조를 뜻함

복합구조(composite structure)를 사용하면 복합객체와 개별객체를 대상으로 똑같은 작업을 적용할 수 있다. (= 복합 객체와 개별 객체를 구분할 필요가 거의 없어짐)

- Client: Component인터페이스를 사용해서 복합 객체 내의 객체들을 조작 가능. 리프와 컴포지트를 동일하게 처리
- Component: 복합 객체 내에 들어있는 모든 객체의 인터페이스를 정의. 복합 객체와 개별 객체가 공통으로 구현해야 할 메서드를 선언
- Leaf: 트리의 말단 요소. 자식이 없음. 그 안에 들어있는 원소의 행동을 정의
- Composite: 자식이 있는 구성요소의 행동을 정의하고 자식 구성 요소를 저장. 다른 구성 요소(리프나 또 다른 컴포지트)를 자식으로 가질 수 있는 복합 객체.
  add(component), remove(component), getChild(int), operation

복합 객체에는 자식들이 들어있고, 그 자식들은 복합객체일수도 있고 아닐 수도(잎일 수도) 있다. 재귀구조로 되어있음.

Leaf와 Node는 각각 역할이 다르지만, 모든 구성 요소에 MenuComponent를 구현해야만 한다
자기 역할에 맞지 않는 상황을 기준으로 예외를 던지는 코드를 기본 구현으로 제공
```cs
// 모든 메소드를 기본적으로 구현해 놓음
public abstract class MenuComponent
{
  public void add(MenuComponent menuComponent)
  {
    throw new unsupportedOperationException();
  }
  public void remove(MenuComponent menuComponent)
  {
    throw new unsupportedOperationException();
  }
  public void getChilde(int i)
  {
    throw new unsupportedOperationException();
  }

  // MenuItem에서 작업을 처리하는 메소드. 이 중 몇개는 Menu에서도 사용
  public string getName()
  {
    throw new unsupportedOperationException();
  }
  public string getDescription()
  {
    throw new unsupportedOperationException();
  }
  public double getPrice()
  {
    throw new unsupportedOperationException();
  }
  public bool isVegetarian()
  {
    throw new unsupportedOperationException();
  }

  // Menu와 MenuItem에서 모두 구현하는 작업용 메소드
  public void print()
  {
    throw new unsupportedOperationException();
  }
}
```

MenuItme구현
```cs
public class MenuItem : MenuComponent
{
  string name;
  string description;
  bool vegetarian;
  double price;

  public MenuItem(string name, string desc, bool vegetarian, double price)
  {
    (...생략...)
  }

  public string getName()
  {
    return name;
  }

  public string getDescription()
  {
    return description;
  }

  public double getPrice()
  {
    return price;
  }

  public bool isVegetarian()
  {
    return vegetarian;
  }

  public void print()
  {
    print(" " + getName());
    if(isVegetarian())
    {
      print("(v)");
    }
    print("," + getPrive());
    print("   --" + getDescription());
  }
}
```

Menu구현
```cs
// 마찬가지로 MenuComponent를 상속
public class Menu : MenuComponent
{
  List<MenuComponent> menuComponents = new();
  string name;
  string description;

  public Menu(string name, string description)
  {
    (...생략...)
  }

  public void add(MenuComponent menuComponent)
  {
    menuComponents.Add(menuComponent);
  }

  public void remove(MenuComponent menuComponent)
  {
    menuComponents.Remove(menuComponent);
  }

  public MenuComponent getChild(int i)
  {
    return menuComponents.get(i);
  }

  public string getName()
  {
    return name;
  }
  public string getDescription()
  {
    return description;
  }

// getPrice()와 isVegetarian()은 어울리지 않는 메소드이므로 구현하지 안흔다.
// Menu에서 이를 호출하려고 하면 에러를 던진다

  public void print()
  {
    print("\n" + getName());
    print(", " + getDescription());
    print("--------------");

    foreach(var menuComponent in menuComponents)
    {
      menuComponent.print(); // MenuItem들을 순환하면서 print 실행
    }
  }
}
```

(클라이언트)테스트 코드
```cs
public class Waitress
{
  MenuComponent allMenus;
  // 최상위 메뉴 구성요소만 넘겨준다
  public Waitress(MenuComponent allMenus)
  {
    this.allMenus = allMenus;
  }

  public void printMenu()
  {
    allMenus.print();
  }

}
```

- 이렇게 되면 한 클래스에서 한 역할이 아니라 두 가지 역할을 맡고 있는 것이 아닌가?
  : 컴포지트 패턴에서는 단일 역할 원칙을 깨고 대신에 투명성을 확보.
- 투명성(Transparency)이란?
  : 어떤 원소가 복합 객체인지 잎인지 클라이언트는 투명하게 알 수 있다
- 상황에 따라서 원칙을 적절하게 사용해야한다
	- 원칙이 디자인에 어떤 영향을 끼칠지 항상 고민하며 적용해야 한다


- 복합구조가 너무 복잡하거나 복합 객체 전체를 도는 데 너무 많은 자원이 필요하다면 복합 노드를 캐싱해두면 도움이 된다.
	- 예를들어 복합객체에 있는 모든  자식이 어떤 계산을 하고 그 계산을 반복계산한다면 계산 결과를 임시로 저장하는 캐시를 만드는 것


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

반복자 패턴
- 컬렉션의 구현 방법을 노출하지 않으면서 집합체  내의 모든 항목에 접근하는 방법을 제공합니다.

컴포지트 패턴
- 객체를 트리구조로 구성해서  부분-전체 계층구조를 구현합니다. 컴포지트 패턴을 사용하면 클라이언트에서 개별 객체와 복합 객체를 똑같은 방법으로 다룰 수 있습니다.


