사용자 정의 및 성능 고려사항으로 인해, UnLua는 기본적으로 다음과 같은 엔진의 일반적인 클래스들을 익스포트했습니다(자세한 내용은 코드를 참조):

#### 기본 타입
 * UObject
 * UClass
 * UWorld
 
#### 일반 컨테이너
 * TArray
 * TSet
 * TMap

#### 수학 라이브러리
 * FVector
 * FVector2D
 * FVector4
 * FQuat
 * FRotator
 * FTransform
 * FColor
 * FLinearColor
 * FIntPoint
 * FIntVector

## 정적 익스포트

UnLua는 타입/멤버 변수/열거형 등을 정적으로 익스포트하는 간단한 방안을 제공합니다.

### 클래스
* 비반사 클래스
```
BEGIN_EXPORT_CLASS(ClassType, ...)
```
 또는
```
BEGIN_EXPORT_NAMED_CLASS(ClassName, ClassType, ...)
```
 **'...'**는 생성자 메서드의 매개변수 목록을 나타냅니다.

* 반사 클래스
```
BEGIN_EXPORT_REFLECTED_CLASS(UObjectType)
```
 또는
```
BEGIN_EXPORT_REFLECTED_CLASS(NonUObjectType, ...)
```
 **'...'**는 생성자 메서드의 매개변수 목록을 나타냅니다.

#### 멤버 변수
```
ADD_PROPERTY(Property)
```
또는 (비트 필드 불린 타입 속성)
```
ADD_BITFIELD_BOOL_PROPERTY(Property)
```

#### 멤버 함수
##### 비정적 멤버 함수
* 간소화 스타일
```
ADD_FUNCTION(Function)
```
또는
```
ADD_NAMED_FUNCTION(Name, Function)
```

* 완전한 스타일
```
ADD_FUNCTION_EX(Name, RetType, Function, ...)
```
또는
```
ADD_CONST_FUNCTION_EX(Name, RetType, Function, ...)
```
**'...'**는 매개변수 타입 목록을 나타냅니다.

##### 정적 멤버 함수
```
ADD_STATIC_FUNCTION(Function)
```
또는
```
ADD_STATIC_FUNCTION_EX(Name, RetType, Function, ...)
```
**'...'**는 매개변수 타입 목록을 나타냅니다.

#### 예제
```
struct Vec3
{
	Vec3() : x(0), y(0), z(0) {}
	Vec3(float _x, float _y, float _z) : x(_x), y(_y), z(_z) {}

	void Set(const Vec3 &V) { *this = V; }
	Vec3& Get() { return *this; }
	void Get(Vec3 &V) const { V = *this; }

	bool operator==(const Vec3 &V) const { return x == V.x && y == V.y && z == V.z; }

	static Vec3 Cross(const Vec3 &A, const Vec3 &B) { return Vec3(A.y * B.z - A.z * B.y, A.z * B.x - A.x * B.z, A.x * B.y - A.y * B.x); }
	static Vec3 Multiply(const Vec3 &A, float B) { return Vec3(A.x * B, A.y * B, A.z * B); }
	static Vec3 Multiply(const Vec3 &A, const Vec3 &B) { return Vec3(A.x * B.x, A.y * B.y, A.z * B.z); }

	float x, y, z;
};

BEGIN_EXPORT_CLASS(Vec3, float, float, float)
	ADD_PROPERTY(x)
	ADD_PROPERTY(y)
	ADD_PROPERTY(z)
	ADD_FUNCTION(Set)
	ADD_NAMED_FUNCTION("Equals", operator==)
	ADD_FUNCTION_EX("Get", Vec3&, Get)
	ADD_CONST_FUNCTION_EX("GetCopy", void, Get, Vec3&)
	ADD_STATIC_FUNCTION(Cross)
	ADD_STATIC_FUNCTION_EX("MulScalar", Vec3, Multiply, const Vec3&, float)
	ADD_STATIC_FUNCTION_EX("MulVec", Vec3, Multiply, const Vec3&, const Vec3&)
END_EXPORT_CLASS()
IMPLEMENT_EXPORTED_CLASS(Vec3)
```

### 전역 함수
```
EXPORT_FUNCTION(RetType, Function, ...)
```
또는
```
EXPORT_FUNCTION_EX(Name, RetType, Function, ...)
```
**'...'**는 매개변수 타입 목록을 나타냅니다.

#### 예제
```
void GetEngineVersion(int32 &MajorVer, int32 &MinorVer, int32 &PatchVer)
{
	MajorVer = ENGINE_MAJOR_VERSION;
	MinorVer = ENGINE_MINOR_VERSION;
	PatchVer = ENGINE_PATCH_VERSION;
}

EXPORT_FUNCTION(void, GetEngineVersion, int32&, int32&, int32&)
```

### 열거형
* 스코프가 없는 열거형
```
enum EHand
{
	LeftHand,
	RightHand
};

BEGIN_EXPORT_ENUM(EHand)
	ADD_ENUM_VALUE(LeftHand)
	ADD_ENUM_VALUE(RightHand)
END_EXPORT_ENUM(EHand)
```

* 스코프가 있는 열거형
```
enum class EEye
{
	LeftEye,
	RightEye
};

BEGIN_EXPORT_ENUM(EEye)
	ADD_SCOPED_ENUM_VALUE(LeftEye)
	ADD_SCOPED_ENUM_VALUE(RightEye)
END_EXPORT_ENUM(EEye)
```