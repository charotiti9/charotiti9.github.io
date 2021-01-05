---
layout: post
title: C# 코루틴 변수 저장
tags: [unity, c#]
description: >
 C#, Unity에서 코루틴을 사용할 때 변수에 저장해서 사용하는 2가지 방법과 이에 따른 주의사항을 알아본다.
comments: true
---

* this unordered seed list will be replaced by toc as unordered list
{:toc}

코루틴을 변수로 저장하고 싶을 때가 있다. 하나의 코루틴 함수를 여러 곳에서 호출하거나, 코루틴을 멈추고 싶을 때 주로 변수로 저장하여 호출하곤 한다.

# 코루틴의 문제점

코루틴을 시작할때(StartCoroutine) 함수를 호출하는 방식은 2가지 있다.  
하지만 함수를 매개변수로 호출을 하면 코루틴을 멈추고 싶을 때(StopCoroutine) 명령어가 제대로 작동하지 않는다.

```c#
    IEnumerator CoroutineMethod()
    {
    	// to do...
    	while(true)
    	{
    		print("Hi");
    		yield return null;
    	}
    }
    
    // - string값으로 코루틴 시작
    StartCoroutine("CoroutineMethod");
    StopCoroutine("CoroutineMethod"); // 멈출 수 있다
    
    // - 함수호출로 코루틴 시작
    StartCoroutine(CoroutineMethod());
    StopCoroutine(CoroutineMethod()); // 멈출 수 없다
    StopCoroutine("CoroutineMethod"); // 멈출 수 없다
```

위처럼 함수호출로 코루틴을 시작하는 경우 멈출 수 없는 문제가 있다.

그럼 계속 string값으로 불러온다면 되지 않냐고 생각할 수도 있다. 하지만 파라메터를 전달해야 할 때 문제가 생긴다.  
혹은 Ctrl+F12 등을 이용하여 어디서 이 함수를 호출했는지 모아서 보고 싶은 경우 아주 귀찮아진다.

```c#
    IEnumerator CoroutineMethod(GameObject obj)
    {
    	// to do...
    	while(true)
    	{
    		obj.transform.position += Vector3.Right * 0.3f * Time.deltaTime;
    		yield return null;
    	}
    }
    
    // - 함수호출로 코루틴 시작
    StartCoroutine(CoroutineMethod(wantedObj));
    StopCoroutine(CoroutineMethod(wantedObj)); // 멈출 수 없다...
```

이럴 때 코루틴을 변수로 저장하여 사용하게 된다.

# 코루틴을 변수로 저장

변수로 저장하는 방법도 2가지가 있다.  
하나는 Coroutine 클래스를 저장하는 방법, 그리고 또 하나는 IEnumerator 인터페이스를 저장하는 방법이다.

## Coroutine 저장

```c#
    IEnumerator CoroutineMethod(GameObject obj)
    {
    	// to do...
    	while(true)
    	{
    		obj.transform.position += Vector3.Right * 0.3f * Time.deltaTime;
    		yield return null;
    	}
    }
	
    // 변수선언
    Coroutine co = null;
    
     // 호출할 때 변수에 저장
     co = StartCoroutine(CoroutineMethod(wantedObj));
     // 중단
     StopCoroutine(co);
```

## IEnumerator 저장

```c#
    IEnumerator CoroutineMethod(GameObject obj)
    {
    	// to do...
    	while(true)
    	{
    		obj.transform.position += Vector3.Right * 0.3f * Time.deltaTime;
    		yield return null;
    	}
    }
    
    // 변수선언
    IEnumerator ie = null;
     
    // 변수에 저장
    ie  = CoroutineMethod(twantedObj);
    // 호출
    StartCoroutine(ie);
    // 중단
    StopCoroutine(ie);
```

### IEnumerator 저장 시 주의사항
IEnumerator로 저장시에는 항상 StartCoroutine 전에 변수에 새롭게 코루틴 함수를 저장해주어야 한다.  
그래야만 새로운 코루틴으로 인식하기 때문이다.  

만약 IEnumerator 변수에 코루틴 함수를 한 번만 저장하게 되면  
StopCoroutine 후 다시 StartCoroutine을 호출하였을 때 중단된 곳에서부터 시작하게 된다.
즉 0~4까지 찍는 코루틴이 있다면 2에서 StopCoroutine이 되었다면, 다시 StartCoroutine을 할 때는 3부터 찍힌다는 것이다.

이게 유용하게 쓰이겠다는 생각이 잠깐 들 수도 있지만 코루틴의 동기성과 yield에 관한 내용을 잘 이해하고 있어야만 한다.  
(그래도 사실 별로 추천하진 않는다...)

예시코드

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CoroutineTest : MonoBehaviour
{
    IEnumerator iEnumerator;
    Coroutine coroutine;


    private void Start()
    {
        // Start에서 1번만 저장하면 Stop->Start 시에 중단된 곳부터 재개
        // 코루틴이 완료된 상태라면 시작조차 하지 않음.
        iEnumerator = IEnumeratorTestMethod();
    }

    IEnumerator IEnumeratorTestMethod()
    {
        for (int i = 0; i < 5; i++)
        {
            print("iEnumerator: " + i);
            yield return new WaitForSeconds(1.0f);
        }
        print("---------End");
    }
    IEnumerator CoroutineTestMethod()
    {
        for (int i = 0; i < 5; i++)
        {
            print("coroutine: " + i);
            yield return new WaitForSeconds(1.0f);
        }
        print("---------End");
    }

    private void OnGUI()
    {
        GUI.Box(new Rect(10, 10, 100, 90), "iEnumerator");
        if (GUI.Button(new Rect(20, 40, 80, 20), "Start"))
        {
            print("Start-------");
            // StartCoroutine 시에 항상 객체를 새로 저장한다면 다른 참조값을 사용하므로 Stop->Start 해도 처음부터 시작
            // 테스트를 원한다면 주석해제 후 플레이.
            //iEnumerator = IEnumeratorTestMethod();
            StartCoroutine(iEnumerator);
        }
        if (GUI.Button(new Rect(20, 70, 80, 20), "Stop"))
        {
            StopCoroutine(iEnumerator);
            print("Stop--------");
        }



        GUI.Box(new Rect(150, 10, 100, 90), "coroutine");
        if (GUI.Button(new Rect(160, 40, 80, 20), "Start"))
        {
            print("Start-------");
            coroutine = StartCoroutine(CoroutineTestMethod());
        }
        if (GUI.Button(new Rect(160, 70, 80, 20), "Stop"))
        {
            StopCoroutine(coroutine);
            print("Stop--------");
        }
    }
}
```