# FAQ 자주 묻는 질문

## ReceiveTick 오버라이드가 작동하지 않나요?

블루프린트에서 Tick 이벤트가 활성화된 경우에만 Lua의 Tick이 실행됩니다. 이는 UE 자체 설계로, Tick을 활성화하는 방법은 두 가지입니다. 하나는 C++에서 `bCanEverTick`을 true로 설정하는 것이고, 다른 하나는 블루프린트에서 `Event Tick` 노드를 생성하는 것입니다. `FKismetCompilerContext::SetCanEverTick()` 함수를 참조하세요.

## Lua 코드가 패키징 후 로드되지 않나요?

"Additional Non-Asset Directories to Package" 항목에 Script 디렉토리를 추가하세요.

## 블루프린트 함수에 공백이 있는 경우 Lua에서 어떻게 호출하나요?

Lua의 네이티브 언어 특성을 사용하여 `[]`를 통해 `function` 객체에 접근한 후 호출할 수 있습니다.

```lua
self["function name with space"]() -- "정적 함수"
self["function name with space"](self) -- "인스턴스 함수"
```

## 새로 생성한 프로젝트에 UnLua를 설치했는데 열었을 때 `could not be compiled. Try rebuilding from source manually` 메시지가 나타나는 이유는?

새 프로젝트 생성 시 블루프린트 프로젝트가 아닌 C++ 프로젝트를 선택하세요.

## UnLua의 바인딩 버튼을 찾을 수 없나요?

블루프린트 에디터 창의 너비가 너무 작아서 바인딩 아이콘이 드롭다운 버튼으로 축소되었습니다. 에디터 창을 최대화해 보세요.

## 블루프린트 메서드를 오버라이드할 때 왜 모두 `Receive`와 같은 접두사를 붙여야 하나요?

블루프린트에서 보는 메서드 이름의 많은 부분이 읽기 친화적으로 처리된 것입니다. `Actor`에서 가장 흔한 `BeginPlay`를 예로 들면, C++에서는 다음과 같습니다:

```cpp
    /** Event when play begins for this actor. */
    UFUNCTION(BlueprintImplementableEvent, meta=(DisplayName = "BeginPlay"))
	void ReceiveBeginPlay();
```

실제로 블루프린트에서 대응되는 것은 `ReceiveBeginPlay` 메서드이며, `DisplayName`을 통해 블루프린트에서 더 친화적으로 보이게 할 뿐입니다.

## `UE.XXX`로 내가 생성한 블루프린트 타입에 접근할 수 없나요?

`2.2.0` 버전부터 `UE.XXX`는 C++ 타입에만 접근을 지원하며, 블루프린트 타입은 먼저 `UE.UClass.Load`로 로드한 후 사용해야 합니다.

## 다중 상속 계층에서 인터페이스 메서드를 오버라이드할 수 없나요?

`BlueprintNativeEvent`와 `BlueprintImplementationEvent`로 표시된 `UFUNCTION`만 오버라이드할 수 있습니다.

## 블루프린트의 일부 로직만 다시 작성할 방법이 있나요?

일부 로직을 함수로 합치고(Collapse To Function), Lua에서 해당 함수를 오버라이드할 수 있습니다.

## 디버깅을 어떻게 사용하나요?

참조 문서: [디버깅](Debugging.md)

## IntelliSense(지능형 제안)을 어떻게 사용하나요?

참조 문서: [지능형 제안](IntelliSense.md)

## Lua 파일 핫 업데이트를 어떻게 지원하나요?

간단한 방법으로는 UnLua의 기본 로딩 메커니즘을 활용할 수 있으며, `FPaths::ProjectPersistentDownloadDir()` 디렉토리의 스크립트를 우선적으로 로드합니다.

Windows 플랫폼을 예로 들면, 패키징 후 프로젝트의 Lua 파일 `require "A.B.C"`는 다음 순서로 로드를 시도합니다:
1. WindowsNoEditor/프로젝트명/Saved/PersistentDownloadDir/Content/Script/A/B/C.lua
2. WindowsNoEditor/프로젝트명/Content/Script/A/B/C.lua

모바일 플랫폼의 다운로드 디렉토리는:
- Android: /storage/emulated/0/Android/data/com.game.xxx/files/Content/Script/A/B.lua
- iOS: App의 Document 디렉토리/PersistentDownloadDir/Content/Script/A/B.lua

사용자 정의 로더 예제를 참조하여 완전히 맞춤형 로딩 전략을 구현할 수도 있습니다.

## `package.path`를 수정했는데 효과가 없는 이유는? 사용자 정의 `require` 검색 디렉토리를 설정할 수 있나요?

UE는 자체 파일 시스템을 가지고 있으므로, 사용자 정의 검색 디렉토리가 필요한 경우 `UnLua.PackagePath`를 수정하여 구현할 수 있습니다. 예를 들어:

```lua
UnLua.PackagePath = UnLua.PackagePath .. ';Plugins/UnLuaExtensions/LuaProtobuf/Content/Script/?.lua'
```

PS: 기본값은 `Content/Script/?.lua;Plugins/UnLua/Content/Script/?.lua`입니다.

## UnLua를 사용하고 있는 제품이 있나요?

텐센트 내부에서 알려진 것만 약 40개 프로젝트가 UnLua를 사용하고 있으며, 외부 프로젝트는 현재 통계를 낼 수 없습니다.