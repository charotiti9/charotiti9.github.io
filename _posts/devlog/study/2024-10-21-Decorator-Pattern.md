---
layout: post
title: 헤드퍼스트 디자인패턴 정리 03 - 데코레이터 패턴
category: devlog
tags: [study, design-pattern]
image: /assets/img/blog/study/DecoratorPattern.png
description: >
 헤드퍼스트 디자인패턴 책을 읽으면서 주요 내용을 정리해놓은 노트를 공개합니다.
comments: true
---

헤드퍼스트 디자인패턴 3장 데코레이터 패턴에 대해 요약정리한 내용입니다.

# 음료 예시
음료를 주문하는 시스템을 만든다고 치자
Beverage 추상 클래스를 상속받아서 각각의 음료를 만든다고 쳤을 때, 음료에 들어가는 옵션들(디카페인, 스팀밀크 추가 등...)이 추가되면 각각의 음료 클래스들이 기하급수적으로 늘어나게 된다.

- 우유가격이 인상된다면?
- 캐러맬을 새로 추가하게 된다면?

디자인 원칙 가운데 2가지를 어기고 있다.

그럼 만약 각 첨가물을 인스턴스 변수와  슈퍼클래스 상속을 사용해서 관리한다면?
milk, soy, mocha, whip 등을 추가했는지 bool로 관리한다고 해보자. 그리고 cost()함수에서 각 첨가물의 가격까지 계산하여 서브클래스에서  cost() 메소드를 오버라이드 할 때 그 기능을 확장해서 음료 가격을 더한다.

```cs
public class Beverage
{
  public double cost()
  {
    double condimentCost = 0.0;
    if(hasMilk())
    {
      condimentCost += milkCost;
    }
    
    (...생략...)

    return condimentCost;
  }
}


public class DarkRoast : Beverage
{
  public double cost()
  {
    return darkRoastCost + base.cose();
  }
}
```


이 프로젝트에서 변경되었을 때 디자인에 영향을 미칠만한 요소
- 첨가물 가격이 바뀔 때마다 기존 코드를 수정해야한다
- 첨가물의 종류가 많아지면 새로운 메소드를 추가해야하고, 슈퍼클래스의 cost() 메소드도 고쳐야한다
- 새로운 음료가 출시될 수 있다. 그 중에는 특정 첨가물이 들어가면 안되는 음료도 있을 수 있다
- 고객이 더블 모카를 주문한다면?

# OCP 살펴보기
우리의 목표는 기존 코드를 건드리지 않고 확장으로 새로운 행동을 추가하는 것.

**디자인 원칙 중 다섯번째**
**'클래스는 확장에는 열려 있어야 하지만 변경에는 닫혀있어야 한다'**

모든 ㅂ분에서 OCP를 준수하는 것은 보통 불가능하다. OCP를 지키다보면 새로운 단계의 추상화가 필요한 경우가 종종 있는데, 추상화를 하다보면 코드가 복잡해진다.
따라서, 가장 바뀔 가능성이 높은 부분을 중점적으로 살피고 OCP를 적용해야한다.

# 데코레이터 패턴 살펴보기
첨가물로 그 음식을 장식(decorate)한다고 생각해보자.
예를들면
- DarkRoast 객체를 가져온다
- Mocha 객체로 장식한다
- Whip 객체로 장식한다
- cost() 메소드를 호출한다
  이 때, 첨가물의 가격을 계산하는 일은 해당 객체에게 위임한다

여기서  '장식한다'라는 것은 '감싼다'의 의미와 같다
- 객체가 장식하고 있는 객체를 반영(mirror)한다
  = 같은 형식을 갖는다
- Mocha도 Beverage의 서브클래스 형식이고 cost() 메소드가 있다
- Whip도 DarkRoast의 형식을 반영하고 cost() 메소드가 있다

여기서 가격을 계산해본다면
- 가장 바깥쪽에 있는 Whip의 cost() 메소드 호출
- Whip는 Mocha의 cost() 메소드 호출
- Mocha는 DarkRoast의 cost() 메소드 호출
- DarkRoast가 Mocha에게 가격을 return
- Mocha는 Whip에게 둘을 더한 가격을 return
- Whip은 최종 가격을 return

정리하자면 다음과 같다
- 데코레이터의 슈퍼클래스는 자신이 장식하고 있는 객체의 슈퍼클래스와 같다
- 한 객체를 여러 개의 데코레이터로 감쌀 수 있다
- 데코레이터는 자신이 감싸고 있는 객체와 같은  슈퍼클래스를 가지고 있기에 원래 객체(싸여있는 객체)가 들어갈 자리에 데코레이터 객체를 넣어도 상관 없다
- 데코레이터는 자신이 장식하고 있는 객체에게 어떤 행동을 위임하는 일 말고도 추가 작업을 수행할 수 있다 (key point!)
- 객체는 언제든지 감쌀 수 있으므로 실행 중에 필요한 데코레이터를 마음대로 적용할 수 있다

# 데코레이터 패턴의 정의
데코레이터 패턴으로 객체에  추가 요소를 동적으로 더할 수 있다. 데코레이터를 사용하면 서브클래스를 만들 때보다 훨씬 유연하게 기능을 확장할 수 있다.

![](/assets/img/blog/study/Pasted%20image%2020240923151936.png)

 
- ConcreteComponent: 새로운 행동을 Component에게 동적으로 추가
- Decorator: 자신이 장식할 구성요소와 같은 인터페이스 또는 추상 클래스를 구현
  각 데코레이터 안에는 Component 객체가 들어있다. 즉, 데코레이터에는 구성 요소의 레퍼런스를 포함한 인스턴스 변수가 있다.
  각 구성요소는 직접 쓰일 수도 있고 데코레이터에 감싸여 쓰일 수도 있다.
- ConcreteDecorator
  :  데코레이터가 새로운 메소드를 추가할 수도 있지만, 일반적으로는 새로운 메소드를 추가하는 대신 Component에 원래 있던 메소드를 (호출하거나 호출한 후에) 별도의 작업으로 처리해서 새로운 기능을 추가한다
	- A: 데코레이터가 감싸고 있는 Component 객체용 인스턴스 변수가 있다
	- B: 데코레이터는 Component의 상태를 확장할 수 있다


# 이 패턴을 Beverage 클래스에 적용시킨다면...
![](/assets/img/blog/study/Pasted%20image%2020240923152906.png)
- Beverage: Component 추상 클래스와 비슷한 개념
- HouseBlend, DarkRoast, Espresso, Decaf: 커피 종류마다 구성요소를 나타내는 구상 클래스
- CondimentDecorator: 첨가물 데코레이터의 인터페이스
- Milk, Mocha, Soy, Whip:  각각 첨가물을 나타내는 데코레이터


**Q. 상속 대신 구성을 사용하라고 하지 않았나?**
CondimentDecorator는 상속을 사용하고 있다.
데코레이터 형식이 그 데코레이터로 감싸는 객체의 형식과 같아야 하므로 상속으로 형식을 맞추고 있다. 하지만 상속을 통해 행동을 물려받지는 않고 있다.
행동은 기본 구성요소와는 다른 데코레이터 등을 인스턴스 변수에 저장하는 식으로 연결한다.

**Q. Beverage 클래스를 왜 인터페이스로 만들지 않고 추상클래스로 만들었나?**
사실 인터페이스를 써도 된다. 하지만 기존 코드를 고치는 일은 될 수 있으면 피하는 것이 좋으니 추상 클래스를 써도 되는 상황이라면 그냥  추상 클래스만 가지고 작업을 하는게 나을 수도 있다.


# 코드로 살펴보기

Beverage 추상 클래스
```cs
public abstract class Beverage
{
  string description = "제목 없음";

  public string getDescription()
  {
    return description;
  }

  public abstract double cost();
}
```


첨가물을 나타내는 추상 클래스
```cs
// Beverage 객체가 들어갈 자리에 들어갈 수 있어야하므로 Beverage 클래스를 확장
public abstract class CondimentDecorator : Beverage
{
 // 데코레이터가 감쌀 음료를 나타내는 Beverage 객체
 // 어떤 음료든 감쌀 수 있도록 슈퍼클래스 유형을 사용한다
  Beverage beverage;

  // 모든 첨가물 데코레이터에 getDescription() 메소드를 새로 구현하도록 함
  public abstract string getDescription();
}
```


실제 음료 클래스
```cs
public class Espresso : Beverage
{
  public Espresso()
  {
    description = "에스프레소";
  }
  
  public double cost()
  {
    return 1.99;
  } 
}
```


첨가물 코드
```cs
// 데코레이터 상속(CondimentDecorator에서는 Beverage를 상속받고 있다)
public class Mocha : CondimentDecorator
{
  public Mocha(Beverage beverage)
  {
    // 감싸고자하는 음료
    this.beverage = beverage;
  }

  public string getDescription()
  {
    return beverage.getDescription() + "모카"; // 첨가되는 아이템도 설명에 추가
  }

  public double cost()
  {
    return beverage.cost() + .20; // 가격 추가
  }
}
```


테스트 코드
```cs
public class StarbuzzCoffee
{
  public static void main(string args[])
  {
    // 아무것도 넣지 않은 에스프레소
    Beverage beverage = new Esprosso();
    // 에스프레소 $1.99
    print($"{beverage.getDescription()} ${beverage.cost()}");

    // 모카2 휘핑1을 추가한 다크로스트
    Beverage beverage2 = new DarkRoast();
    beverage2 = new Mocha(beverage2);
    beverage2 = new Mocha(beverage2);
    beverage2 = new Whip(beverage2);
    // 다크로스트 모카, 모카, 휘핑크림 $1.49
    print($"{beverage2.getDescription()} ${beverage2.cost()}"); 
  }

}
```


# Q&A

Q. 데코레이터로 감싸고 나면 그 커피가 하우스 블렌드인지 다크로스트인지 알 수 없다. 
A. 맞다. 구상 구성요소로 어떤 작업을 처리하는 코드에 데코레이터 패턴을 적용하면 제대로 작동하지 않는다. 구상 구성요소로 돌아가는 코드를 만들어야한다면 데코레이터 패턴 사용을 재고해보아야 한다.

Q. 데코레이터를 빼먹는 실수를 한다면?
A. 실제로는 팩토리나 빌더같은 다른 패턴으로 데코레이터를 만들고 사용한다. 나중에 그런 패턴을 배우다 보면 데코레이터로 장식된 구상 구성 요소는 캡슐화가 잘 되어 있어서 별로 걱정하지 않아도 된다.

Q. '모카, 휘핑크림, 모카'가 아니라 '휘핑크림, 더블모카' 라는 식으로 출력하고 싶다면 가장 바깥쪽에 있는 데코레이터가 안에 있는 데코레이터를 모두 알아야하지 않나?
A. 데코레이터는 감싸고 있는 객체에 행동을 추가하는 용도로 만든다. 만약 여러 단계의 데코레이터를 파고 들어가서 어떤 작업을 해야한다면, 데코레이터 패턴이 만들어진 의도에 어긋난다. 단, 마지막에 만들어진 설명을 파싱해서 문자열을 바꿔주면 된다.

Q. 만약 톨, 그란데, 벤티 사이즈 개념을 도입한다면? 그리고 그에따라 첨가물 가격도 다르다면?
A. setSize()와 getSize() 함수 추가.
```cs
public abstract class Beverage
{
  public enum Size {TALL, GRANDE, VENTI};
  Size size = Size.TALL;

  string description = "제목 없음";

  public string getDescription()
  {
    return description;
  }

  public void setSize(Size size)
  {
    this.size = size;
  }

  public Size getSize()
  {
    return this.size;
  }

  public abstract double cost();
}
```

CondimentDecorator에 사이즈 추가
```cs
public abstract class CondimentDecorator : Beverage
{
  Beverage beverage;
  public abstract string getDescription();

  // 추가
  public Size getSize()
  {
    return beverage.getSize();
  }
}
```

첨가물 코드에 cost 계산식 변경
```cs
public class Mocha : CondimentDecorator
{
  public Mocha(Beverage beverage)
  {
    this.beverage = beverage;
  }

  public string getDescription()
  {
    return beverage.getDescription() + "모카";
  }

  public double cost()
  {
    double cost = beverage.cost();

    // 사이즈마다 cost 더하기
    if(beverage.getSize() == Size.TALL)
    {
      cost += .10;
    }
    else if (beverage.getSize() == Size.GRANDE)
    {
      cost += .15;
    }
    else if (beverage.getSize() == Size.VENTI)
    {
      cost += .20;
    }

    return cost;
  }
}
```

# 단점에 대하여 
- 디자인을 유연하게 만들어주긴 하지만 자잘한 클래스가 엄청나게 추가되곤 함
	- 따라서 남들이 보기에 이해하기 힘든 디자인이 됨
	- 그럼에도 불구하고 좋은 디자인이긴 함
- 특정 형식에 의존하는 클라이언트 코드를 가지고 와서 제대로 생각해보지 않도 데코레이터 패턴을 적용하는 사람이 많다
	- 데코레이터를 끼워넣어도 클라이언트는 데코레이터를 사용하고 있다는 사실을 알 수 없다
	- 하지만 특정 형식에 의존하는 코드에 데코레이터를 적용하면 모든게 엉망이 됨
	- 데코레이터를 끼워넣을 때 이러한 문제를 조심해야함
- 데코레이터를 도입하면 구성요소를 초기화하는데 필요한 코드가 훨씬 복잡해짐
	- 팩토리와 빌더 패턴을 사용해보자


# 마무리
객체지향 원칙
- 바뀌는 부분은 캡슐화한다
- 상속보다는 구성을 활용한다
- 구현보다는 인터페이스에 맞춰서 프로그래밍한다
- 상호작용하는 객체 사이에서는 가능하면 느슨한 결합을 사용해야한다
- 클래스는 확장에 열려 있어야 하지만 변경에는 닫혀 있어야 한다(OCP)

데코레이터 패턴
: 객체에 추가 요소를 동적으로 더할 수 있습니다. 데코레이터를 사용하면 서브클래스를 ㅁ나들 때보다 훨씬 유연하게 기능을 확장할 수 있습니다.