[TOC]

# 1. 개요
UnLua는 기능이 풍부하고 고도로 최적화된 UE 스크립트 솔루션입니다. 개발자는 Lua를 사용하여 게임 로직을 작성할 수 있으며, Lua의 핫 리로드 특성을 활용하여 훨씬 빠른 게임 로직 반복 속도를 얻을 수 있습니다. 본 문서는 UnLua의 주요 기능과 기본 프로그래밍 패턴을 소개합니다.

---

# 2. Lua와 엔진의 바인딩
UnLua는 Lua와 엔진 레이어를 바인딩하는 두 가지 방법을 제공합니다: 정적 바인딩과 동적 바인딩:

## 정적 바인딩

#### C++
UCLASS는 `IUnLuaInterface` 인터페이스를 구현하고, `GetModuleName_Implementation()`에서 Lua 파일 경로를 반환하면 됩니다.

![CPP_UNLUA_INTERFACE](../Images/cpp_unlua_interface.png)

#### 블루프린트
블루프린트는 `UnLuaInterface` 인터페이스를 구현하고, `GetModuleName()`에서 Lua 파일 경로를 반환하면 됩니다.

![BP_UNLUA_INTERFACE](../Images/bp_unlua_interface.png)
![BP_GETMODULENAME](../Images/bp_getmodulename.png)

## 동적 바인딩
동적 바인딩은 런타임에 스폰된 Actor와 Object에 적용됩니다.

#### Actor
```cpp
local Proj = World:SpawnActor(ProjClass, Transform, ESpawnActorCollisionHandlingMethod.AlwaysSpawn, self, self.Instigator, "Weapon.BP_DefaultProjectile_C")
```
`Weapon.BP_DefaultProjectile_C`는 [Lua 모듈 경로](#Lua모듈경로)입니다.

#### Object
```lua
local ProxyObj = NewObject(ObjClass, nil, nil, "Objects.ProxyObject")
```
`Objects.ProxyObject`는 [Lua 모듈 경로](#Lua모듈경로)입니다.

## Lua 모듈 경로
정적 바인딩과 동적 바인딩 모두 Lua 모듈 경로를 지정해야 합니다. 이는 **{프로젝트 디렉토리}/Content/Script**의 Lua 파일에 대한 **상대 경로**로, 반각 마침표 **.** 로 구분됩니다.

---

# 3. Lua에서 엔진 호출
UnLua는 Lua측에서 엔진에 접근하는 두 가지 방법을 제공합니다:
1. 반사 시스템을 사용한 동적 익스포트
2. 반사 시스템을 우회하여 클래스, 멤버 변수, 멤버 함수, 전역 함수, 열거형 등을 정적으로 익스포트

## 반사 시스템을 사용한 동적 익스포트
동적 익스포트는 반사 시스템을 사용하여 코드가 더 깔끔하고 간결하며, 대량의 글루 코드가 필요하지 않습니다.

### UCLASS 접근
```lua
local Widget = UE.UWidgetBlueprintLibrary.Create(self, UE.UClass.Load("/Game/Core/UI/UMG_Main_C"))
```

**UE**는 전역 객체로, 그 속성에 접근할 때 지연 로딩 방식으로 엔진 타입을 가져옵니다.

**UWidgetBlueprintLibrary**는 UCLASS입니다. Lua 클래스 이름 규칙은 다음과 같아야 합니다:

`C++ 접두사` + `클래스 이름` + ``_C``

여기서 접미사 `_C`는 블루프린트 타입을 나타냅니다. 예: `AActor` (네이티브 클래스), `ABP_PlayerCharacter_C`(블루프린트 클래스)

### UFUNCTION 접근
```lua
Widget:AddToViewport(0)
```
`AddToViewport`는 `UUserWidget`의 UFUNCTION이고, `0`은 함수 매개변수입니다.
UFUNCTION(`BlueprintCallable` 또는 `Exec`로 표시됨)의 특정 매개변수에 기본값이 있는 경우, Lua 코드에서 전달을 생략할 수 있습니다:
```lua
Widget:AddToViewport()
```

#### 출력값 처리
출력값에는 **비상수 매개변수**와 **반환 매개변수**가 포함되며, 모두 **기본 타입(bool, integer, number, string)**과 **비기본 타입(userdata)**으로 구분됩니다.

##### 비상수 매개변수
###### 기본 타입
![OUT_PRIMITIVE_TYPES](../Images/out_primitive_types.png)

Lua 코드:
```lua
local Level, Health, Name = self:GetPlayerBaseInfo()
```

###### 비기본 타입
![OUT_NON_PRIMITIVE_TYPES](../Images/out_non_primitive_types.png)

Lua에서 호출하는 두 가지 방법:

```lua
local HitResult = FHitResult()
self:GetHitResult(HitResult)
```
또는:
```lua
local HitResult = self:GetHitResult()
```
첫 번째 방법은 C++와 더 유사하며, 특히 루프에서 여러 번 호출할 때 두 번째 방법보다 훨씬 효율적입니다.

##### 반환 매개변수
###### 기본 타입
![RET_PRIMITIVE_TYPES](../Images/return_primitive_types.png)

Lua 코드:
```lua
local MeleeDamage = self:GetMeleeDamage()
```

###### 비기본 타입
![RET_NON_PRIMITIVE_TYPES](../Images/return_non_primitive_types.png)

Lua에서 호출하는 세 가지 방법:
```lua
local Location = self:GetCurrentLocation()
```
또는:
```lua
local Location = UE.FVector()
self:GetCurrentLocation(Location)
```
또한:
```lua
local Location = UE.FVector()
local LocationCopy = self:GetCurrentLocation(Location)
```
첫 번째가 가장 직관적이지만, 나머지 두 방법은 루프에서 여러 번 호출할 때 성능이 더 좋습니다. 세 번째는 다음과 같습니다:
```lua
local Location = UE.FVector()
self:GetCurrentLocation(Location)
local LocationCopy = Location
```

#### Latent 함수
Latent 함수는 개발자가 동기 코드 스타일로 비동기 로직을 작성할 수 있게 해줍니다. **Delay**는 전형적인 Latent 함수입니다:

![LATENT_FUNCTION](../Images/latent_function.png)

Lua 코루틴에서 Latent 함수를 호출할 수 있습니다:

```lua
coroutine.resume(coroutine.create(function(GameMode, Duration)
    UE.UKismetSystemLibrary.Delay(GameMode, Duration)
end), self, 5.0)
```

### USTRUCT 접근
```lua
local Position = UE.FVector()
```
**FVector**는 USTRUCT입니다.

### UPROPERTY 접근
```lua
local Position = FVector()
Position.X = 256.0
```
**X**는 **FVector**의 UPROPERTY입니다.

### 델리게이트

다음 예제에서 첫 번째 매개변수는 `UObject`로, 이 델리게이트 바인딩의 생명주기를 나타냅니다. 즉, 객체가 무효해진 후(예: 가비지 컬렉션됨) 해당 콜백도 함께 무효가 됩니다.

따라서 비즈니스 코드에서는 객체가 파괴될 때 모든 델리게이트를 수동으로 언바인딩하는 것이 필수는 아닙니다(물론 바인딩과 언바인딩을 대응시키는 것은 좋은 코딩 습관입니다).

#### 단일 캐스트 델리게이트 Delegate
`self.Track`은 `FTimelineFloatTrack`입니다
```lua
-- 바인딩
self.Track.InterpFunc:Bind(self, self.OnZoomInOutUpdate)

-- 언바인딩
self.Track.InterpFunc:Unbind(self, self.OnZoomInOutUpdate)

-- 실행
self.Track.InterpFunc:Execute(0.5)
```

#### 멀티 캐스트 델리게이트 MulticastDelegate
`self.Button`은 `UButton`입니다
```lua
-- 콜백 추가
self.Button.OnClicked:Add(self, self.OnClicked_ExitButton)

-- 콜백 제거
self.Button.OnClicked:Remove(self, self.OnClicked_ExitButton)

-- 모든 콜백 정리
self.Button.OnClicked:Clear()

-- 브로드캐스트로 모든 콜백 트리거
self.Button.OnClicked:Broadcast()
```

#### 할당과 매개변수 전달
```lua

-- 할당 (바인딩과 동일)
self.Track.InterpFunc = { self, self.OnZoomInOutUpdate }

-- 델리게이트 매개변수 전달
UE.UKismetSystemLibrary.K2_SetTimerDelegate({ self, self.OnZoomInOutUpdate }, 1, true)
```

### 열거형 UENUM
```lua
-- C++ 타입
print(UE.EAttachmentRule.SnapToTarget)

-- 블루프린트 타입 (UUserDefinedEnum)
local MyEnum = UE.UObject.Load("/Game/Enums/MyEnum")
print(MyEnum.Value1)

-- 사용자 정의 충돌 열거형 타입
print(UE.EObjectTypeQuery.Player)
print(UE.ETraceTypeQuery.Weapon)
```

[사용자 정의 충돌 열거형](./CollisionEnum.md)은 구성된 이름으로 열거형 값에 직접 접근할 수 있습니다.

---

# 4. 엔진에서 Lua 호출
UnLua는 블루프린트와 유사한 솔루션을 제공하여 C++/스크립트 경계를 넘나들며, C++/블루프린트 코드를 통해 Lua를 호출할 수 있습니다.

## 블루프린트 이벤트 대체
모든 **블루프린트 이벤트**의 구현을 Lua 코드로 오버라이드하여 대체할 수 있습니다:

* **'BlueprintImplementableEvent'**로 표시된 UFUNCTION
* **'BlueprintNativeEvent'**로 표시된 UFUNCTION
* 블루프린트에서 정의된 **모든** 이벤트/함수

### 예제 (반환값이 없는 블루프린트 이벤트)

![BP_IMP_EVENT](../Images/bp_imp_event.png)

Lua에서 직접 대체할 수 있습니다:
```lua
function BP_PlayerController_C:ReceiveBeginPlay()
    print("ReceiveBeginPlay in Lua!")
end
```

### 예제 (반환값이 있는 블루프린트 이벤트)

![BP_IMP_EVENT_RET](../Images/bp_imp_event_return_values.png)

Lua에서 반환값과 Out 매개변수를 순서대로 반환할 수 있으며, Out 매개변수를 반환하지 않으면 기본적으로 전달된 매개변수를 반환합니다

```lua
function BP_PlayerCharacter_C:GetCharacterInfo(HP, Position, Name)
    return true, 99, FVector(128.0, 128.0, 0.0), "Marcus"
end
```

## 애니메이션 이벤트 대체

![ANIM_NOTIFY](../Images/anim_notify.png)

```lua
function ABP_PlayerCharacter_C:AnimNotify_NotifyPhysics()
    UBPI_Interfaces_C.ChangeToRagdoll(self.Pawn)
end
```
Lua 함수 이름은 **'AnimNotify_'** + **{이벤트 이름}** 형식이어야 합니다

## 입력 이벤트 대체
![ACTION_AXIS_INPUTS](../Images/action_axis_inputs.png)

참조 예제: [03_BindInputs](../../Content/Script/Tutorials/03_BindInputs.lua)

## 복제 이벤트 대체
참조 예제: [10_Replications](../../Content/Script/Tutorials/10_Replications.lua)

## 대체된 함수 호출
Lua로 원래 구현을 오버라이드한 경우에도 `Overridden`을 통해 원래 함수에 접근할 수 있습니다.
```lua
function BP_PlayerController_C:ReceiveBeginPlay()
    local Widget = UWidgetBlueprintLibrary.Create(self, UClass.Load("/Game/Core/UI/UMG_Main"))
    Widget:AddToViewport()
    self.Overridden.ReceiveBeginPlay(self)
end
```
`self.Overridden.ReceiveBeginPlay(self)`는 원래 블루프린트에서 구현된 `ReceiveBeginPlay`를 호출합니다.

## C++에서 Lua 함수 호출

* 전역 함수
```
template <typename... T>
FLuaRetValues Call(lua_State *L, const char *FuncName, T&&... Args);
```

* 전역 테이블 내의 함수
```
template <typename... T>
FLuaRetValues CallTableFunc(lua_State *L, const char *TableName, const char *FuncName, T&&... Args);
```

---

# 5. 가비지 컬렉션

UnLua 2.2 버전부터 Lua 환경은 더 이상 `UObject`에 참조를 추가하지 않으므로, 객체가 Lua측에서 참조되어 계속 해제되지 않는 메모리 누수 문제를 더 이상 걱정할 필요가 없습니다.

Lua측에서 UEGC에 의해 회수된 객체에 접근하면 `attempt to read property on released object`와 유사한 오류가 표시됩니다.

이러한 오류가 발생하면 객체가 UE 참조 체인에 있도록 보장해야 하며, 다음과 같은 방법을 사용할 수 있습니다:

1. 객체를 UPROPERTY에 할당 (C++ 또는 블루프린트 속성 모두 가능)
2. 객체를 AddToRoot
3. `UnLua.Ref(Object)`를 사용하여 [참조 프록시](#참조프록시) 생성

## 참조 프록시

Lua측에서 특정 `UObject`에 대한 참조를 유지해야 하지만 할당할 적절한 참조 체인이 없는 경우, `UnLua.Ref` 인터페이스를 사용하여 참조 프록시를 생성할 수 있습니다:

```lua
local MyClass = UE.UClass.Load("/Game/MyClass")

-- RefProxy 객체가 luagc되지 않는 한 MyClass 객체에 대한 참조를 계속 유지
RefProxy = UnLua.Ref(MyClass)

-- nil로 할당한 후 참조 프록시는 다음 luagc에서 회수되어 UObject에 대한 참조를 해제
RefProxy = nil
```

동일한 `UObject`에 대해 `UnLua.Ref`를 여러 번 호출하면 매번 동일한 참조 프록시가 반환되므로 중복 참조 문제를 걱정할 필요가 없습니다.

또한 참조 프록시의 lua측 생명주기를 활용하여 `UObject`에 대한 참조 생명주기를 동기화할 수 있습니다:

```lua
-- UMyWidget 타입이 있다고 가정 (다른 선언 생략)
function UMyWidget:Construct()
    self.MyClass = UE.UClass.Load("/Game/MyClass")
    self.ClassRef = UnLua.Ref(self.MyClass)
end

-- 인스턴스를 생성하고 AddToViewport
-- MyWidget이 UE 참조 체인에 있으므로 이 UI는 UE에 의해 회수되지 않음
-- lua측에 바인딩된 table도 회수되지 않으므로 MyClass도 회수되지 않음
local MyWidget = NewObject(UMyWidget)
MyWidget:AddToViewport()

-- UE 참조 체인에서 제거되면 MyWidget이 UEGC됨
-- 이로 인해 lua측 table이 LuaGC될 수 있고, ClassRef도 GC될 수 있음
-- 따라서 참조 프록시가 Lua의 UObject 참조를 UE측에 동기화한다고 이해할 수 있음
MyWidget:RemoveFromViewport()

```

때로는 로직상 프록시 객체를 null로 설정하여 참조를 해제하는 것을 잊을 수 있거나, 즉시 참조를 해제하고자 할 수 있습니다(예: "게임 진입 시 모든 로비 UI 해제"와 같은 명확한 시점).

`UnLua.Unref(Object)`를 사용하여 강제로 참조를 해제할 수 있습니다.

참조를 해제한 후 UEGC가 이 객체를 정상적으로 회수할 수 있습니다. 일단 회수되면 Lua측에서 이 객체에 다시 접근할 때 오류가 표시되어 해제 로직을 편리하게 보완할 수 있습니다.

# 6. 기타

[정적 익스포트](./StaticExportBinding.md)
[사용자 정의 Lua 템플릿 생성](./CustomTemplate.md)