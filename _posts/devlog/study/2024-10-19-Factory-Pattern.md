---
layout: post
title: 헤드퍼스트 디자인패턴 정리 04 - 팩토리 패턴
category: devlog
tags: [study, design-pattern]
image: /assets/img/blog/study/FactoryPattern.png
description: >
 헤드퍼스트 디자인패턴 책을 읽으면서 주요 내용을 정리해놓은 노트를 공개합니다.
comments: true
---

헤드퍼스트 디자인패턴 4장 팩토리 패턴에 대해 요약정리한 내용입니다.

객체의 인스턴스를 만드는 작업이 항상 공개되어야  하는 것은 아니며, 오히려 모든 것을 공개했다가는 결합 문제가 생길 수 있다. 

# new 연산자
new 연산자를 사용하면  '구상 클래스'의 인스턴스가 만들어진다. = 인터페이스가 아닌 특정 구현을 사용하는 것.
앞에서는 구상클래스를 바탕으로 코딩하면 나중에 코드를 수정해야할 가능성이 커지고 유연성이 떨어진다고 배웠다.

`Duck duck = new MallardDuck();`
이처럼 인터페이스를 써서 코드를 유연하게 만드려고 해도 결국 구상 클래스의 인스턴스를 만들어야한다.

이러면 여러 조건으로 각각의 구상 클래스를 만들어야할 때 변경이 커진다.
```cs
Duck duck;

// 오리를 나타내는 클래스는 여러가지 있지만,
// 컴파일 하기 전에는 어떤 것의 인스턴스를 만들어야 하는지 알 수 없다
if(picnic)
{
  duck = new MallardDuck();
}
else if(hunting)
{
  duck = new DecoyDuck();
}
else if(inBathTub)
{
  duck = new RubberDuck();
}
```

하지만 언젠가는 객체를 생성해야한다.
인터페이스를  바탕으로 만들어진 코드는 다형성 덕분에 특정 인터페이스만 구현하면 사용할 수 있다. 반대로 구상 클래스를 많이 사용하면 변경에 닫혀있는 코드가 된다. 구상 형식을  써서 확장 해야할 때는 어떻게 해서든 **다시 열 수 있게 만들어야한다.**

바뀌는 부분을 찾아내서 바뀌지 않는 부분과 분리하면 된다.

# 예시로 알아보기
피자가게를 운영한다고 쳐보자. 객체 생성 부분은 늘 계속 바뀌게 될 것이다. (메뉴가 추가되거나, 삭제될 때) 이  부분을 피자를 만드는 일만 처리하는 객체에 따로 넣는다.이러한 객체를 '팩토리'라고 부르자.

이렇게 했을 때 팩토리를 사용하는 클라이언트가 매우 많을 경우에도 도움이 된다.

# 심플 팩토리 

심플 팩토리는 정적 팩토리(static factory)로 정적 메소드로 선언하는 디자인도 있다. 정적 메소드를 쓰면 객체 생성 메소드를 실행하려고  객체의 인스턴스를 만들지 않아도 된다. 하지만 서브클래스를 만들어서 객체 생성 메소드의 행동을 변경할 수 없다는 단점도 있다.

아래는 심플팩토리를 사용하는 예시이다.

```cs
public class PizzaStore
{
  SimplePizzaFactory factory;
  public PizzaStore(SimplePizzaFactory factory)
  {
    this.factory = factory;
  }

  public Pizza orderPizza(string type)
  {
    Pizza pizza;
    // new 연산자 대신 팩토리 객체에 있는 create 메소드 사용
    // 더이상 구상 클래스의 인스턴스를 만들 필요가 없다.
    pizza = factory.createPizza(type);

    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();

    return pizza;
  }
}
```

아래는 심플팩토리의 예시이다. 심플팩토리는 엄밀히 말해서 패턴은 아니고 프로그래밍에서 자주 쓰이는 관용구에 가깝다. 

```cs
public class SimplePizzaFactory
{
  public Pizza createPizza(string type)
  {
    Pizza pizza = null;
    if(type.equals("cheese"))
    {
      pizza = new cheesePizza();
    }
    else if(type.equals("pepperoni"))
    {
      pizza = new PepperoniPizza();
    }
    // (...)
    return pizza;
  }
}
```

아래와같은 클래스들로 작동한다.
- PizzaStore: 팩토리를 사용하는 클라이언트.
- SimplePizzaFactory: 피자객체를 생성하는 팩토리(유일하게 구상 Pizza 클래스를 직접 참조)
- Pizza: 팩토리에서 만드는 피자 추상 클래스
- CheesePizza, VeggiePizza, ClamPizza, PepperoniPizza
  : 팩토리에서 생산. 각 피자는 Pizza 추상클래스를 구현하는 구상 클래스. 

# 팩토리 메소드 패턴
PizzaFactory를 더 다양하게 확장해본다. 지점이 더 늘어났다고 한다면,
뉴욕스타일 피자 팩토리, 시카고 피자 팩토리... 등등
```cs
NYPizzaFactory nyFactory = new NYPizzaFactory();
PizzaStore nyStore = new PizzaStore(nyFactory);
nyStore.orderPizza("Veggie");
```

하지만 더 제대로 관리해야한다.
굽는 방식이 달라진다거나, 종종 피자를 자르는 것을 까먹기도 한다. 심지어 이상하게 생긴 피자 상자를 쓴다.
이 문제를 해결하려면 PizzaStore와 피자 제작 코드 전체를 하나로 묶어주는 프레임워크를 만들어야한다. 

## 피자가게 프레임워크 만들기
PizzaStore에 추상메소드로 createPizza() 코드를 넣는다
```cs
public abstract class PizzaStore()
{
  public Pizza orcderPizza(string type)
  {
    Pizza pizza;
    pizza = createPizza(type);

    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();

    return pizza;
  }

  // 팩토리 객체 대신에 이 추상메소드를 사용한다
  abstract Pizza createPizza(string type);
}
```


이제 PizzaStore의 서브클래스에서 createPizza() 메소드를 구현한다
변경되는 부분은 모두 createPizza() 메소드 안에 넣는 것이다.
- NYStylePizzaStore, ChicagoStyePizzaStore...
  : createPizza()는 추상메소드이기 때문에 서브클래스를 만들때 이 메소드를 반드시 구현한다

```cs
public class NYPizzaStore : PizzaStore
{
  Pizza createPizza(string item)
  {
    if(item.equlas("cheese"))
    {
      return new NYStlyeCheesePizza();
    }
    else if(...)
  }
}
```

이렇게 되었을때 PizzaStore는 어떤 종류의 피자를 받는지 알 수 없고 결정하지도 않는다.
'어떤 서브클래스를 선택했느냐'에 따라서 피자의 종류가 결정된다.

=> 팩토리 메소드는 객체 생성을 서브클래스에 캡슐화. 이렇게 했을 때 슈퍼클래스에 있는 클라이언트 코드와 서브클래스에 있는 객체 생성 코드를 분리할 수 있다.

## 피자 클래스
```cs
public abstract class Pizza
{
  string name;
  string dough;
  string sauce;
  List<string> toppings = new();

  void prepare()
  {
    foreach(string topping in toppings)
    {
      print(" " + topping);
    }
  }
  
  void bake()
  {
    print("굽기")
  }
  
  void cut()
  {
    print("사선으로 자르기")
  }

  (...)

  public string getName()
  {
    return name;
  }
}
```

이제 서브클래스를 만든다

```cs
public class NYStyleCheesePizza : Pizza
{
  public NYStyleCheesePizza()
  {
    name = "뉴욕스타일 소스와 치즈피자";
    douch = "씬 크러스트 도우";
    sauce = "마리나라 소스"
    toppings.add("잘게 썬 레지아노  치즈");
  }

  // cut 메소드를 오버라이드 할 수도 있다
  void cut()
  {
    print("네모난 모양으로 피자 자륵기")
  }
}
```

## 피자 테스트코드
```cs
public class PizzaTestDrive
{
  public static void main(string[] args)
  {
    PizzaStore nyStore = new NYPizzaStore();
    PizzaStore chicagoStore = new ChicagoPizzaStore();

    Pizza pizza = nyStore.orderPizza("cheese");
    print(pizza.getName()); // 뉴욕 스타일 소스와 치즈 피자
    pizza = chicagoStore.orderPizza("cheese")
    print(pizza.getName()); // 시카코 스타일 딥 디쉬 치즈 피자
  }
}
```

## 팩토리 메소드 패턴 살펴보기
서브클래스에서 어떤 클래스를 만들지 결정함으로써 객체 생성을 캡슐화.

- 생산자(Creator) 클래스
	- PizzaStore
	- NYPizzaStore, ChicagoPizzaStore...
- 제품(Product) 클래스
	- Pizza
	- NYStyleCheesePizza, NYStylePepperoniPizza, ChicagoStyleCheesePizza, ChicagoStylPepperoniPizza...


## 팩토리 메소드 패턴의 정의

> [!팩토리 메소드 패턴]
> 객체를 생성할 때 필요한 인터페이스를 만듭니다. 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정합니다. 팩토리 메소드 패턴을 사용하면 클래스 인스턴스 마드는 일을 서브클래스에게 맡기게 됩니다.

- 구상 생산자 클래스가 하나밖에 없더라도 팩토리 메소드 패턴은 충분히 유용함
	- 제품을 생산하는 부분과 사용하는 부분을 분리할 수 있기 때문에
	- 다른 제품을 추가하거나 구성을 변경하더라도 Creator클래스가 ConcreteProduct와 느슨하게 연결되어있기 때문에 건들일 필요가 없다.
- 팩토리 메소드와 생산자 클래스는 꼭 추상으로 선언할 필요는 없다. 몇몇 간단한 구상 제품은 기본 팩토리 메소드를 정의해서 Creator의 서브클래스 없이 만들 수 있다.
- 심플팩토리메소드와  팩토리메소드패턴의 차이
	- 팩토리 메소드 패턴에서 피자를 리턴하는 클래스가 서브클래스라는 점을 빼면 거의 같지만
	- 간단한 팩토리는 일회용 처방이고 팩토리 메소드 패턴은 여러번 재사용이 가능한 프레임워크를 만들 수 있다


# 객체 의존성 살펴보기

## 의존성 뒤집기 원칙

**디자인 원칙 중 여섯번째**
**'추상화된 것에 의존하게 만들고, 구상 클래스에 의존하지 않게 만든다'**

'구현보다는 인터페이스에 맞춰서 프로그래밍한다'는 원칙과 비슷하긴 하지만 의존성 뒤집기 원칙에서는 추상화를 더 많이 강조한다.
고수준 구성요소가 저수준 구성 요소에 의존하면 안되며, 항상 추상화에 의존하게 만들어야 한다.

PizzaStore는 고수준 구성요소, Pizza 클래스는 저수준 구성요소라고 할 수 있다.
PizzaStore가 모든 종류의 Pizza에 의존하고 있다면 심하게 의존적이라고 할 수 있다.
그래서 Pizza라는 추상 클래스를 만들어서 PizzaStore는 Pizza 추상 클래스 하나에만 의존한 것이다.

근데 왜 의존성 '뒤집기' 원칙이라는 걸까?
객체 지향 디자인을 할 때 일반적으로 생각하는 방법과는 반대로 뒤집어서 생각해야하기 때문.
고수준 모듈과 저수준 모듈이 둘 다 하나의 추상 클래스에 의존하고 있는 것을 생각해보자.

## 의존성 뒤집기 원칙을 지키는 방법

- 변수에 구상 클래스의 레퍼런스를 저장하지 않는다
	- 팩토리를 써서 구상 클래스의 레퍼런스를 별도로 가져가자
- 구상 클래스에서 유도된 클래스를 만들지 않는다
	- 인터페이스나 추상 클래스처럼 추상화된 것으로부터 클래스를 만들자
- 베이스 클래스에 이미  구현되어있는 메소드를 오버라이드하지 않는다
	- 이미 구현되어있는 메소드를 오버라이드하면 베이스 클래스가 제대로 추상화 되지 않는다
	  베이스 클래스에서 메소드를 정의할 때는 서브클래스에서 공유할 수 있는 것만 정의해야 한다

# 추상 팩토리 패턴

각 피자가게들의 원재료가 서로 다를 때를 가정해본다.
뉴욕과 캘리포니아의 피자가게는 `FreshClams`를 사용하는데 시카고의 경우 `FrozenClams`를 사용한다.

일단 원재료들을 생산하는 팩토리용 인터페이스를 정의해보자.

```cs
public interface PizzaIngredientFactory
{
  // 재료마다 하나씩 클래스를 만들어야 한다
  public Dough createDough();
  public Sauce createSauce();
  (...)
}
```

이를 뉴욕원재료팩토리로 구현한다면
```cs
public class NYPizzaIngredientFactory : PizzaIngredientFactory
{
  public Dough createDough()
  {
     return new ThinCrustDough();
  }
  public Sauce createSauce()
  {
    return new MArinaraSauce();
  }
  (...)
}
```

이에 맞춰서 Pizza클래스도 변경해준다
```cs
public abstract class Pizza
{
  string name;

  Douch dough;
  Sauce sauce;
  (...)

  // prepare 메소드를 추상 메소드로 만듦
  abstract void prepare();

  // 이하로는 변경 없음
  void bake()
  {
    print("굽기");
  }
  
  (...)
}
```

이제 각 피자의 구상클래스를 만든다
```cs
public class CheesePizza : Pizza
{
  PizzaIngredientFactory ingredientFactory;

  public CheesePizza(PizzaIngredientFactory ingredientFactory)
  {
     this.ingredientFactory = ingredientFactory;
  }

  void prepare()
  {
    dough = ingredientFactory.createDough();
    sauce = ingredientFactory.createSauce();
    cheese = ingredientFactory.createCheese();
  }
}
```


이제 올바른 피자를 만드는지 확인해보면 된다
```cs
public class NYPizzaStore :  PizzaStore
{
  protected Pizza createPizza(string item)
  {
    Pizza pizze = null;
    PizzaIngredientFactory ingredientFactory = new NYPizzaIngredientFactory;

    if(item.equal("cheese"))
    {
      pizze = new CheesePizza(ingredientFactory);
    }
    if(item.equal("clam"))
    {
      pizze = new ClamPizza(ingredientFactory);
    }
    (...)
    return pizza;
  }
}
```


## 추상 팩토리 패턴의 정의

>[! 추상 팩토리 패턴]
> 구상 클래스에 의존하지 않고도 서로 연관되거나 의존적인 객체로 이루어진  제품군을 생산하는 인터페이스를 제공한다. 구상 클래스는 서브 클래스에서 만든다.

- 클라이언트에서 추상 인터페이스로 일련의 제품을 공급받을 수 있다
- 클라이언트와 팩토리에서 생산되는 제품의 분리가 가능하다 (실제로 어떤 제품이 생산되는지는 알 필요 없음)

#  추상팩토리패턴과 팩토리메소드패턴
- 추상 팩토리 패턴에서 메소드가 팩토리  메소드로 구현되는 경우도 종종 있다

- 팩토리 메소드 패턴
	- 상속으로 객체를 만든다
	- 클래스를 확장하고 팩토리 메소드를 오버라이드 해야 함
	- 한 가지 제품만 생산
	- 클라이언트 코드와 인스턴스를 만들어야할 구상 클래스를 분리시켜야 할 때
- 추상 팩토리 패턴
	- 객체 구성(composition)으로 객체를 만든다
	- 제품군을 만드는 추상 형식을 제공. 제품이 생산되는 방법은 이 형식의 서브클래스에서 정의.
	- 인터페이스를 바꿔야만 새로운 제품을 추가 가능
	- 클라이언트에서 서로 연관된 일련의 제품(=제품군)을 만들어야 할 때


## 예시로 알아보기
- PizzaStore: 한 제품을 생산하는데 필요한 추상인터페이스 제공 (createPizza())
  => 지역마다 다른 제품을 만들 수 있어야 하므로 **팩토리 메소드 패턴** 사용 
- NYPizzaStore, ChicagoPizzaStore: 각 서브클래스에서 어떤 클래스의 인스턴스를 만들지 결정
  => 내부에 createPizza() 라는 팩토리 메소드 존재
- Pizza: PizzaStore에서 생산하는 제품의 추상 형식
- NYStyleCheesePizza, ChicagoStyleClamPizza...
  : 서브 클래스의 인스턴스는 **팩토리 메소드**에 의해서 만들어진다
- PizzaIngredientFactory: 제품군을 생산하는 추상 인터페이스를 제공(createDough()...)
  => 제품군(원재료들)을 만들어야하기 때문에 **추상 팩토리 패턴** 사용
- NYPizzaIngredientFactory, ChicagoPizzaIngredientFactory...
  : 각 구상 서브클래스에서 제품군을 생산,  
  추상 팩토리에서 제품을 생산하는 메소드를 **팩토리 메소드 패턴**으로 구현

# 마무리

객체지향 원칙
- 바뀌는 부분은 캡슐화한다
- 상속보다는 구성을 활용한다
- 구현보다는 인터페이스에 맞춰서 프로그래밍한다
- 상호작용하는 객체 사이에서는 가능하면 느슨한 결합을 사용해야한다
- 클래스는 확장에 열려 있어야 하지만 변경에는 닫혀 있어야 한다(OCP)
- ==추상화된 것에 의존하게 만들고 구상 클래스에 의존하지 않게 만든다==


아래 두 가지 패턴은
- 객체 생성을 캡슐화
- 클라이언트와 구상 클래스가 서로 분리된 유연한 디자인을 구현할 수 있게 도와줌


추상 팩토리 패턴
- 구상 클래스에 의존하지 않고도 서로 연관되거나 의존적인 객체로 이루어진 제품군을 생성하는 인터페이스를 제공. 구상 클래스는 서브클래스에서 만든다

팩토리 메소드 패턴
-  객체를 생성할 때 필요한 인터페이스를 만든다. 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정한다. 팩토리 메소드를 사용하면 인스턴스 만드는 일을 서브클래스에게 맡길 수 있다
