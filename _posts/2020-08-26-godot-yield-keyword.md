---
title: Godot에서 yield 키워드 사용하기
date: 2020-08-26
tag: [Godot, 게임개발]
summary: Godot 엔진에서 yield를 어떻게 사용하는지 알아보자.
---



## 코루틴 (Coroutine)

**코루틴**이란, 함수의 실행을 잠시 일시 정지시켰다가 재개할 수 있는 기능이다. Python을 쓰던 사람이라면, generator의 개념에 익숙할 텐데, 제너레이터가 바로 코루틴의 일종이다. Godot 3에서는 `yield` 키워드를 통해 코루틴을 사용할 수 있다. 하지만, 그 행동이 비직관적인 데다가 매뉴얼도 제대로 작성되어있지 않아 이해하는 데 어려움이 많다. 이 글에서는 어떻게 `yield` 키워드를 잘 사용할 수 있는지 이야기해보려고 한다.



## `yield()`

GDScript의 `yield()` 함수는 기본적으로 다음과 같이 정의된다.

```
yield(Object object = null, String signal = "")
```

따라서, `yield()`는 Godot에서 두 가지 용법으로 사용된다. 첫째는 아무런 인자를 주지 않았을 때이다. 다음 코드를 보자.

```go
func _ready():
    my_func_one()

func my_func_two():
    print("Three")
    yield()
    print("Four")

func my_func_one():
    print("One")
    var y = my_func_two()
    print("Two")
    y.resume()
```

`var y = my_func_two()`라인에서 `my_func_two()`가 호출되는 것까지는 그냥 `return`과 동일하다. 하지만 `my_func_two()` 블럭 안에서 `yield()`를 만났을 때, `my_func_two()`의 상태는 `GDScriptFunctionState`로 저장되어 반환된다. (혹은, _주도권_이 caller에게 넘어간다.) 이 상태는 `resume()` 함수를 통해 재개시킬 수 있다. 따라서, 위 코드의 출력은

```
One
Three
Two
Four
```

가 된다.



## `yield(object, signal)`

`yield`에 `signal`을 결합시킬 때 이 키워드의 진정한 힘이 나타난다. (signal에 대해서는 언젠가 다룰 기회가 있을 것이다.) 이 경우, `yield`는 `object`에 주도권을 넘겨준 뒤, `signal` 시그널이 감지될 때까지 함수의 실행을 일시 정지시킨다. 이를 활용한 대표적인 예제가 타이머이다.

```go
func _ready():
    yield(get_tree().create_timer(1.0), "timeout")
    print("One")
```

위 코드에서 `get_tree().create_timer(1.0)`은 최상위 트리에다가 1.0초짜리 타이머를 추가하는 함수(=오브젝트)이다. 이 함수는 지정된 시간이 끝나면 `timeout` 시그널을 방출한다. 따라서, 위 코드를 실행하면 1초 뒤 One이 출력된다. 만약 `yield`가 없었더라면, 1초의 타이머는 `_ready()` 함수의 실행과 병렬적으로 일어나므로, 즉시 One이 출력된 후 타이머가 종료되었을 것이다.

또한, 모든 `yield` 키워드는 함수의 상태, `GDScriptFunctionState`가 파괴되었을 때 자동으로 `completed` 시그널을 방출한다. 공식 도큐먼테이션에 있는 코드를 살펴보자면

```go
func _ready():
    yield(countdown(), "completed")
    print('Ready')

func countdown():
    print(3)
    yield(get_tree().create_timer(1.0), "timeout")
    print(2)
    yield(get_tree().create_timer(1.0), "timeout")
    print(1)
    yield(get_tree().create_timer(1.0), "timeout")
```

`_ready()`의 첫 줄은 `countdown()`을 코루틴으로 호출하고 있다. `countdown()` 함수는 우리가 배운 대로 3, 2, 1을 각각 1초의 간격을 두고 출력한다. `countdown()`의 마지막 `yield`문이 종료되면, `countdown()`은 값을 리턴하고 파괴되며, 이 때 `completed` 시그널을 방출한다. 그 다음에야 Ready가 출력된다.

여기서 헷갈릴 만한 지점은, `yield`의 기본 행동은 여기서도 유지된다는 점이다. 즉, `countdown()`의 각 `yield(object, singal)` 함수는 object에 주도권을 넘겨줌과 _동시에_ `_ready()`에 `GDScriptFunctionState`를 반환한다. 만약 `countdown()`을 코루틴을 사용하지 않고 그냥 호출한다고 하자.

```go
func _ready():
    countdown()
    print('Ready')

func countdown():
    print(3)
    yield(get_tree().create_timer(1.0), "timeout")
    print(2)
    yield(get_tree().create_timer(1.0), "timeout")
    print(1)
    yield(get_tree().create_timer(1.0), "timeout")
```

`_ready()`는 먼저 `countdown()`을 호출하고, `countdown()`은 3을 출력한 뒤, 첫 번째 `yield`문을 만난다. 이 때 `countdown()`은 자신의 상태를 `_ready()`에 반환함과 동시에 `get_tree().create_timer(1.0)`에게 주도권을 넘겨준다. 따라서, `_ready()`와 `countdown()`은 이 시점 이후부터는 동시에 병렬적으로 실행되게 된다. 따라서 출력은 다음과 같다.

```
3
Ready
2
1
```



## Godot 4.0

올해 안에 나올 Godot 4.0에서는 `yield` 키워드가 없어지고 `await` 키워드가 추가된다고 한다. 이는 C# 등의 다른 언어와 통일성을 갖는 동시에 많은 혼란을 줄여줄 것으로 예상되는 환영할 만한 변화 같다.