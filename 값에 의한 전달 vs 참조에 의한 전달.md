[[05. 함수]]

## 값 타입(Value Type)과 참조 타입(Reference Type)

| 구분          | 값 타입 (`int`, `float`, `struct` 등) | 참조 타입 (`class`, `array`, `List<T>` 등) |
| ----------- | --------------------------------- | ------------------------------------- |
| 전달 방식       | 값 자체가 복사됨                         | 참조(주소)가 복사됨                           |
| 원본 영향       | ❌ 메서드 안에서 변경해도 원본 영향 없음           | ✅ 참조된 대상 내부를 변경하면 원본 영향 있음            |
| 변경하려면     . | `ref` 키워드 필요                      | 참조 내부를 변경할 수 있음, 참조 자체 변경은 `ref` 필요   |

---



## 🧠 값 타입 예시: `int`, `struct`

```csharp
void ChangeValue(int x)
{
    x = 10;
}

int a = 5;
ChangeValue(a);
Console.WriteLine(a);  // 출력: 5 (원본은 변경되지 않음)
```

## 📦 참조 타입 예시: `class`, `List<T>`, `array`

```csharp
void ChangeList(List<int> list) 
{
	list[0] = 100;  // 원본 리스트의 요소가 변경됨 
}  
List<int> numbers = new List<int> { 1, 2, 3 }; ChangeList(numbers); Console.WriteLine(numbers[0]);  // 출력: 100`
```

> 단, 참조 자체를 새로 할당하면 원본에는 영향 없음

```csharp
void ReplaceList(List<int> list) 
{     
list = new List<int> { 9, 9, 9 };  // 참조 복사본만 바뀜 
}```

---

## 🧱 struct (값 타입)과 ref

```csharp
struct Point 
{
	public int X;     
	public int Y; 
}  

void ChangeStruct(Point p) 
{     
	p.X = 999;  // 복사본만 바뀜 
}

Point pt = new Point { X = 1, Y = 2 }; 
ChangeStruct(pt); Console.WriteLine(pt.X);  // 출력: 1 (원본은 그대로)  

/**********************************************************************/

void ChangeStructRef(ref Point p) 
{     
	p.X = 999; 
}  

ChangeStructRef(ref pt); 
Console.WriteLine(pt.X);  // 출력: 999 (ref 사용으로 원본 변경)
```

---

## 📚 배열은 참조 타입!

- 배열은 `class`처럼 참조 타입
- **요소 변경은 원본에 영향 있음**
- **배열 전체 참조를 바꾸려면 `ref` 필요**
```

```csharp

복사편집

`void ChangeElement(int[] arr) {     arr[0] = 42; }  int[] nums = { 1, 2, 3 }; ChangeElement(nums); Console.WriteLine(nums[0]);  // 출력: 42`

csharp

복사편집

`void ReplaceArray(int[] arr) {     arr = new int[] { 9, 9, 9 };  // 참조 복사본만 변경됨 }  ReplaceArray(nums); Console.WriteLine(nums[0]);  // 출력: 42 (여전히 이전 배열)`

csharp

복사편집

`void ReplaceArrayRef(ref int[] arr) {     arr = new int[] { 8, 8, 8 }; }  ReplaceArrayRef(ref nums); Console.WriteLine(nums[0]);  // 출력: 8 (ref로 참조 자체 변경됨)`

---

## ✅ 정리 요약

- ✔ **값 타입**은 기본적으로 복사되므로 원본 변경 불가
- ✔ **참조 타입**은 내부 요소 변경 가능, 참조 자체를 바꾸려면 `ref` 필요
- ✔ 배열도 참조 타입! 요소는 바로 변경 가능하지만 배열 자체를 바꾸려면 `ref` 사용

