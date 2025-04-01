[[05. 함수]]

c#에서 함수인자에 앞에 ref 넣는거랑 out넣는거는 사실상 같은 기능 아니야?

###### ChatGPT의 말:

겉으로 보기엔 비슷하지만, C#에서 `ref`와 `out`은 중요한 차이점이 있습니다.

### `ref`와 `out`의 공통점

- 둘 다 **참조로 전달**하므로, 함수 내부에서 값을 변경하면 호출한 곳에도 반영됨.
- 값 타입(예: `int`, `float`, `struct` 등)을 **복사하지 않고 직접 변경**할 수 있음.

### 차이점

|구분|`ref`|`out`|
|---|---|---|
|초기화 여부|**반드시** 호출 전에 초기화되어 있어야 함.|초기화되지 않아도 됨 (함수 내부에서 반드시 초기화해야 함).|
|함수 내부에서의 의무|함수 내부에서 꼭 값을 변경할 필요는 없음.|함수 내부에서 반드시 값을 할당해야 함.|
|주 사용 사례|값을 변경하고 다시 사용해야 할 때 (`TryParse` 같은 경우).|함수에서 여러 값을 반환할 때 (다중 리턴 대용).|

---

### 예제 코드 비교

#### `ref` 예제

csharp

복사편집

`void ModifyRef(ref int x) {     x += 10; }  int a = 5; ModifyRef(ref a); Console.WriteLine(a); // 15 (값이 변경됨)`

- `a`는 `ref`를 통해 전달되므로 함수 내에서 변경된 값이 그대로 적용됨.
- 하지만 `ModifyRef`를 호출하기 전에 `a`는 **반드시 초기화**되어 있어야 함.

#### `out` 예제

csharp

복사편집

`void ModifyOut(out int x) {     x = 10;  // 반드시 값 할당 필요! }  int b; ModifyOut(out b); Console.WriteLine(b); // 10 (초기값 없이 사용 가능)`

- `b`는 함수 호출 전에 초기화되지 않았지만, `ModifyOut` 내부에서 반드시 할당되므로 사용 가능.
- **반드시 함수 내부에서 값을 할당해야 하는 규칙**이 있음.

---

### 어떤 걸 언제 써야 할까?

- **기존 값을 변경해야 한다면 → `ref`**  
    (예: `int.Parse` 대신 `TryParse`처럼 값을 검사하고 변경)
- **초기화되지 않은 변수를 함수에서 할당해줘야 한다면 → `out`**  
    (예: `TryParse` 같은 경우 `out`을 사용하여 변환된 값을 반환)

---

### `ref`와 `out`을 함께 쓰는 예제

csharp

복사편집

`bool TryDivide(int dividend, int divisor, out int result) {     if (divisor == 0)     {         result = 0;         return false;     }     result = dividend / divisor;     return true; }  int number; if (TryDivide(10, 2, out number)) {     Console.WriteLine($"Result: {number}"); // 5 } else {     Console.WriteLine("Cannot divide by zero!"); }`

- `out`을 사용해서 함수 내부에서 `result` 값을 반드시 할당하도록 강제함.

---

### 결론

- **`ref`는 값이 미리 초기화되어 있어야 하며, 변경이 가능함.**
- **`out`은 초기화되지 않은 변수를 함수에서 할당하는 용도로 사용됨.**
- **사용 목적이 다르므로, 동일한 기능이라고 보기 어렵다.**