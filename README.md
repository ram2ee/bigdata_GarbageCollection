# 가비지 컬렉션(Garbage Collection)
### ■ 가비지컬렉션이란?
가비지 컬렉션(Garbage Collection)은 메모리 관리 방법 중 하나로 프로그램 실행 중 동적으로 할당된 메모리 중에서 더 이상 사용되지 않는 객체들을 자동으로 탐지하여 해제하는 메모리 관리 기법이다. 

가비지 컬렉션은 메모리 누수(memory leak)와 같은 문제를 방지하고 개발자가 명시적으로 메모리를 관리하는 부담을 줄여준다.

### ■ 메모리 누수 
 메모리 누수란 프로그램에서 동적으로 할당한 memory를 해제하지 않아 메모리가 계속해서 쌓이는 상황을 말한다. 즉 더 이상 필요하지 않은 메모리 영역을 프로그램이 반환하지 않고 유지하게 되는 것.<br>
 정상적으로 반환되지 않은 메모리가 계속 누적되면 프로그램에 할당할 수 있는 메모리가 부족해지면서 프로그램이 비정상적으로 작동하거나 크래시가 발생할 수 있다.

### ■ 가비지컬렉션의 장/단점
<b>장점</b>
- <b>메모리 관리의 편의성</b> : 가비지컬렉션은 개발자가 명시적으로 메모리를 할당하거나 해제할 필요가 없어 메모리 관리가 간편해진다. 객체의 수명 주기와 메모리 할당 및 해제를 신경 쓰지 않아도 되므로 개발자가 코드작성에 집중할 수 있게 된다.<br><br>
- <b>메모리 누수 (Memory Leak) 방지</b> : 가비지컬렉션은 더 이상 참조되지 않는 객체를 감지하고 자동으로 메모리를 해제한다. 이로써 메모리 누수를 방지하고 프로그램의 안전성을 향상시킨다.<br><br>
-  <b>동적 메모리 할당의 유연성</b> : 객체가 동적으로 생성되고 제거되는 경우에도 메모리 관리를 자동으로 처리하여 프로그램의 유연성과 확장성을 높여준다.<br><br>
- <b>프로그램의 성능 향상</b> : 가비지컬렉션은 효율적인 메모리 관리를 통해 프로그램의 성능을 향상시킬 수있다. 메모리 할당 및 해제에 따른 오버헤드를 최소화하고, 불필요한 메모리 조각화를 방지하여 메모리 사용을 최적화할 수 있게 된다.<br><br>

이처럼 가비지컬렉션은 개발자의 생산성과 프로그램의 성능 및 안전성을 향상시켜주는 효과를 가져온다.
하지만 가비지 컬렉션을 사용하는 것은 몇 가지 고려해야할 점도 있다.

<b>단점</b>
- <b>일시적인 작업 중단</b> : 가비지컬렉션 작업을 수행하기 위해 프로그램 실행이 잠시 중단될 수 있는데, 이는 실시간 시스템이나 반응성이 중요한 애플리케이션을 실행중일 때는 문제가 될수도 있다.<br><br>
- <b>예측 어려움</b> : 가비지컬렉션은 필요한 상황에 자동으로 수행되기 때문에 객체의 해제 시점을 예측하기 어렵다. 따라서 프로그램이 메모리를 많이 사용하는 상황에서는 가비지컬렉션작업이 발생하여 성능에 영향을 줄 수도 있다.<br><br>
-  <b>시스템 자원 소모</b> : 가비지 컬렉션은 프로그램의 실행 시간 이외에도 CPU나 메모리등의 자원을 소모하기도 한다.<br><br>

이러한 단점들도 있지만, 이것들을 무시할 만큼 가비지컬렉션의 장점이 크기 때문에 개발자는 애플리케이션의 특성과 성능 요구사항을 고려하며 효율적인 메모리 관리를 하기 위해 최적의 방법으로 가비지컬렉션을 사용해야 한다.

### ■ 가비지컬렉션 대상

실질적으로 객체들은 Heap영역에서 생성되고 Method, Stack Area 등 Root 영역에서는 생성된 객체의 주소만 참조하는 형식으로 구성된다.

![image](https://github.com/ram2ee/bigdata_GarbageCollection/assets/134396201/0c280b4f-2204-42ed-a73d-f2093b0d2219)

하지만 어떠한 이유로 위 그림의 빨간색 객체처럼 Heap영역에서 어디서도 참조되지 않는 객체들이 발생하게 된다. 이러한 객체들을 Unreachable하다고하며 이러한 객체들이 가비지 컬렉션의 대상이 된다.
<pre><code><b>Reachable</b> : 객체가 참조되고 있는 상태<br>
<b>Unreachable</b> : 객체가 참조되고 있지 않은상태 (GC대상)
</code></pre>


### ■ Mark And Sweep 알고리즘
Mark And Sweep 알고리즘은 가장 일반적으로 사용되는 가비지 컬렉션 알고리즘으로 아래있는 그림과 같이 세 단계로 구성된다. 

![image](https://github.com/ram2ee/bigdata_GarbageCollection/assets/134396201/928b0ed5-31bb-4c1a-9289-900079a6a596)

- 첫 번째 단계인 <b>Mark 단계</b>에서는 가비지컬렉션 루트 순회를 통해 시작점으로부터 도달 가능한 모든 객체를 찾아 표시(Mark)한다. 이를 위해 객체의 표시 비트를 설정하거나, 다른 데이터 구조를 사용한다.<br><br>
- 두 번째 단계인 <b>Sweep 단계</b>에서는 메모리에서 표시되지 않은 객체, 즉 Unreachable 객체들을 Heap에서 제거(sweep) 한다.<br><br>
- 마지막 <b>Compact 단계</b>에서는 Sweep 후에 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 압축한다. (가비지 컬렉터 종류에 따라 하지 않는 경우도 있음)

### ■ 가비지컬렉션 코드작성에 주의해야할 점
<b> 1. 순환 참조 주의</b> : 두 개 이상의 객체가 서로를 참조하는 경우, 이 객체들은 도달 가능한 상태로 유지되어 가비지로 처리되지 않는다.

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

# 순환 참조 예제
node1 = Node(1)
node2 = Node(2)

node1.next = node2
node2.next = node1

# 가비지 컬렉션에 의해 해제되지 않음
```

이를 해결하기 위해서는 약한 참조(week reference)를 사용하거나 객체 참조를 명시적으로 해제해야 한다.<br>

<b> 2. 메모리 사용 최소화</b> : 가비지컬렉션은 불필요한 객체를 제거하여 메모리를 최적화하는 역할을 한다. 너무 많은 객체를 생성하면 가비지컬렉션 작업이 자주 발생하여 성능에 영향을 줄 수 있으므로, 메모리 사용을 최소화 하는 것이 중요하다.

```python
# 객체 생성을 최소화하는 예제
def process_data(data):
    # 작업 수행
    print("Processing data:", data)

# 매번 새로운 리스트 생성하지 않고 기존 리스트 재활용
data_list = [1, 2, 3, 4, 5]

for data in data_list:
    process_data(data)
```

<b>3. 컨텍스트 관리</b> : 가비지컬렉션은 객체의 수명을 추적하기 때문에 객체의 사용이 끝난 후에는 참조를 해제해야한다 

```python
# 파일 처리를 위한 컨텍스트 관리자 사용 예제
with open("data.txt", "r") as file:
    data = file.read()
    print(data)

# with 블록을 벗어나면 파일 핸들이 자동으로 닫힘
```

### ■ 메모리 누수(memory leak)

```python
class Person:
    def __init__(self, name):
        self.name = name

def create_person():
    person = Person("John")
    return person

def main():
    while True:
        person = create_person()
        print(person.name)

main()
```

위의 코드에서 ‘creat_person( )' 함수는 'Person' 인스턴스를 생성하고 반환한다. 이 인스턴스는 'person' 변수로 할당되어 'main( )' 함수 내에서 사용되고 있다.<br>
하지만 <code>'person'변수가 계속해서 새로운 'Person' 인스턴스를 참고</code>하고 있는데, 이렇게 되면 이전에 생성된 'Person' 인스턴스들은 더 이상 도달 가능한 객체가 아니지만, 가비지컬렉션으로 삭제되지 않고 메모리에 계속 남아있게 된다. 이것을 <code>메모리 누수(memory leak)</code> 이 발생한다고 본다. <br>
 가비지컬렉션은 도달 불가능한 객체들만을 삭제할 수 있는데 위의 코드에서는 이전에 생성된 'Person' 인스턴스들도 계속해서 'person'변수에 의해 참조되고 있으므로 도달 가능한 객체로 간주하여 삭제하지 않는다는 것이다.<br><br>
  이러한 memory leak을 해결하기 위해서는 <code>불필요한 객체 참고를 해제</code>해 주어야한다. <br>
  위의 코드는 'person'변수가 필요 없어지는 시점에서 'del person'을 사용하여 문제가 되는 객체 참조를 해제할 수 있다.

```python
def main():
    while True:
        person = create_person()
        print(person.name)
        del person  # 불필요한 객체 참조 해제

main()
```

위의 코드를 사용하므로써 이전에 생성되었던 'Person' 인스턴스들은 더 이상 도달 가능한 객체가 아니게되고, 이로서 가비지 컬렉션에 의해서 삭제될 수 있게 된다.
