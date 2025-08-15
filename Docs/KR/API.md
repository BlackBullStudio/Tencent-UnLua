# UnLua API

## C++ API

### 전역 변수
* `UnLua::GLuaSrcRelativePath` Content 디렉토리에 대한 Lua 소스 파일의 상대 경로

* `UnLua::GLuaSrcFullPath` Lua 소스 파일이 위치한 절대 경로

* `FLuaContext::GLuaCxt` 전역 Lua 컨텍스트

### 전역 함수
네임스페이스: UnLua
* `Call` 특정 전역 `function` 호출

* `CallTableFunc` 전역 `table` 내의 특정 `function` 호출

* `Get` Lua 스택의 특정 인덱스 위치에서 지정된 타입의 데이터 가져오기

* `Push` / PushXXX Lua 스택에 지정된 타입의 데이터 푸시

* `IsType` Lua 스택의 지정된 인덱스 위치에 있는 객체 타입 판별

* `CreateState` `GLuaCxt`에서 `lua_State` 생성

* `GetState` `GLuaCxt`에서 `lua_State` 가져오기

* `Startup` UnLua 환경 시작

* `Shutdown` UnLua 환경 종료

* `LoadFile` 지정된 상대 경로에서 Lua 파일을 로드하지만 실행하지 않음

* `RunFile` 지정된 상대 경로에서 Lua 파일 실행

* `LoadChunk` Lua Chunk를 로드하지만 실행하지 않음

* `RunChunk` 지정된 Lua Chunk 실행

* `ReportLuaCallError` Lua 오류 보고, 사용자 정의 오류 처리 델리게이트가 등록되어 있으면 해당 델리게이트만 실행, 그렇지 않으면 UnLua 기본 `UE_LOG` 출력 사용

* `GetStackVariables` 현재 스택의 모든 관련 변수, `upvalue` 정보 가져오기

* `GetLuaCallStack` 현재 Lua 호출 스택의 문자열 가져오기

### 전역 델리게이트
FUnLuaDelegates
* `OnLuaStateCreated` Lua 가상 머신이 생성될 때 트리거, 해당 `lua_State`를 가져올 수 있음

* `OnLuaContextInitialized` `FLuaContext` 초기화 완료 후 트리거

* `OnPreLuaContextCleanup` `FLuaContext` 정리 시작 전 트리거

* `OnPostLuaContextCleanup` `FLuaContext` 정리 완료 후 트리거

* `OnPreStaticallyExport` `FLuaContext`가 `lua_State`에 정적 익스포트 타입을 등록하기 전 트리거

* `OnObjectBinded` 특정 `UObject`가 Lua와 바인딩될 때 트리거

* `OnObjectUnbinded` 특정 `UObject`의 Lua 바인딩이 해제될 때 트리거

* `ReportLuaCallError` UnLua에 사용자 정의 Lua 오류 처리 델리게이트 등록 지원, Lua 오류 발생 시 이 델리게이트 호출

* `ConfigureLuaGC` UnLua에 사용자 정의 LuaGC 구성 델리게이트 등록 지원, UnLua 기본값을 덮어써서 `lua_gc`에 전달되는 매개변수 구성

* `LoadLuaFile` UnLua에 사용자 정의 Lua 파일 로더 델리게이트 등록 지원, 사용자 정의 Lua 파일 로딩 메커니즘 구현

### 인터페이스
* `UUnLuaInterface` Lua에 바인딩될 객체를 표시하고 식별하는 데 사용되는 UnLua의 핵심 인터페이스

### 클래스
* `FLuaContext` 전체 UnLua 런타임 환경 컨텍스트 캡슐화

* `FLuaIndex` Lua 스택 인덱스의 간단한 래퍼

* `FLuaTable` Lua측 `table` 데이터 타입의 래퍼, Lua의 "`[]`"와 유사한 접근 메커니즘 제공, 지정된 `function` 호출도 지원

* `FLuaValue` Lua의 다양한 값 타입에 대한 래퍼

* `FLuaFunction` Lua측 `function` 타입의 래퍼, 전역 또는 지정된 `table` 내의 `function`을 가져와서 호출하는 기능 제공

* `FLuaRetValues` Lua측에서 반환된 값의 래퍼

* `FCheckStack` Lua 스택 밸런스 검사 보조

* `FAutoStack` Lua 스택 인덱스 자동 복구

* `FExportedEnum` Lua측에 열거형을 등록하여 익스포트하는 인터페이스 제공

## Lua API

### 전역 테이블
* `UE` `WITH_UE4_NAMESPACE` 스위치가 활성화된 경우에만 존재, C++측 클래스에 접근하기 위한 루트 객체로 사용. 스위치가 비활성화된 경우, C++에 접근할 때 `_G`를 루트 객체로 직접 사용할 수 있음. 타입은 사용될 때만 Lua에 등록되므로 한 번에 너무 많이 로드하여 성능 문제가 발생할 걱정은 없음

### 전역 함수
* `RegisterEnum` 열거형을 Lua에 수동으로 등록

* `RegisterClass` 클래스를 Lua에 수동으로 등록

* `GetUProperty` 특정 `UObject`의 `UProperty` 가져오기

* `SetUProperty` 특정 `UObject`의 `UProperty`를 지정된 값으로 설정

* `LoadObject` `UObject` 로드, 다음과 같음: `UObject.Load("/Game/Core/Blueprints/AI/BehaviorTree_Enemy.BehaviorTree_Enemy")`

* `LoadClass` `UClass` 로드, 다음과 같음: `UClass.Load("/Game/Core/Blueprints/AICharacter.AICharacter_C")`

* `NewObject` 지정된 Class, Outer(선택사항), Name(선택사항)에 따라 `UObject` 생성

* `UEPrint` `UE_LOG`의 래퍼, `UKismetSystemLibrary::PrintString`도 사용하여 출력

### 인스턴스 함수
* `Initialize` 임의의 객체가 Lua에 바인딩될 때 이 초기화 함수가 호출됨

### 델리게이트
* `Bind` 현재 `FScriptDelegate` 인스턴스에 콜백 바인딩

* `Unbind` 현재 `FScriptDelegate` 인스턴스에서 콜백 언바인딩

* `Execute` `FScriptDelegate`의 모든 콜백 수동 실행

### UnLua.lua
`require "Unlua"` 후에 다음 전역 함수들이 생성됨:
* `Class` OOP 개념을 모방하여 캡슐화, 상속 메커니즘을 제공하는 "클래스" table 생성

* `print` Lua 네이티브 `print`를 `UEPrint`로 덮어씀

## 매크로 정의
다음 매크로 정의는 `UnLua.Build.cs`를 통해 수정할 수 있음:
* `AUTO_UNLUA_STARTUP` UnLua 환경을 자동으로 시작할지 여부, 기본값 **활성화**.

* `WITH_UE4_NAMESPACE` 엔진측 타입에 대한 접근 진입점으로 통합된 `UE` 네임스페이스 `table`을 제공할지 여부, 기본값 **활성화**.

* `SUPPORTS_RPC_CALL` `_RPC` 원격 메서드 지원을 제공할지 여부, 기본값 **활성화**.

* `SUPPORTS_COMMANDLET` `commandlet` 환경에서 UnLua 지원을 제공할지 여부, 기본값 **활성화**.

* `ENABLE_TYPE_CHECK` 타입 검사를 활성화하여 `UFunction` 호출 시 매개변수 타입 유효성 검사를 도와줄지 여부, 기본값 **활성화**.

* `UNLUA_ENABLE_DEBUG` UnLua의 자세한 디버그 로그를 출력할지 여부, 기본값 **비활성화**.