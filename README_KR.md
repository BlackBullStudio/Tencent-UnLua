![LOGO](./Docs/Images/UnLua.png)

[![license](https://img.shields.io/badge/license-MIT-blue)](https://github.com/Tencent/UnLua/blob/master/LICENSE.TXT)
[![release](https://img.shields.io/github/v/release/Tencent/UnLua)](https://github.com/Tencent/UnLua/releases)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/Tencent/UnLua/pulls)

# 개요
**UnLua**는 UE를 위한 고도로 최적화된 **Lua 스크립팅 솔루션**입니다. UE의 프로그래밍 패턴을 따르며, 기능이 풍부하고 학습하기 쉬우며, UE 프로그래머가 학습 비용 없이 사용할 수 있습니다.

# UE에서 Lua 사용하기
* 모든 UCLASS, UPROPERTY, UFUNCTION, USTRUCT, UENUM에 직접 접근, 글루 코드 불필요.
* 블루프린트에서 정의된 구현 교체 ( Event / Function ).
* 각종 이벤트 알림 처리 ( Replication / Animation / Input ).

더 자세한 기능 소개는 [기능 목록](Docs/CN/Features.md)을 참조하세요.

# 최적화 특성
* UFUNCTION 호출, 영구 매개변수 캐싱, 최적화된 매개변수 전달, 최적화된 비상수 참조 및 반환값 처리 포함.
* 컨테이너 클래스 접근 (TArray, TSet, TMap), 엔진과 동일한 메모리 레이아웃, Lua Table과 컨테이너 간 변환 불필요.
* 효율적인 구조체 생성, 접근, GC.
* 커스텀 정적 내보내기 클래스, 멤버 변수, 멤버 함수, 전역 함수, 열거형 지원.

# 플랫폼 지원
* 실행 플랫폼: Windows / Android / iOS / Linux / OSX
* 엔진 버전: Unreal Engine 4.17.x - Unreal Engine 5.x

**주의**: 4.17.x와 4.18.x 버전은 Build.cs에 일부 수정이 필요합니다.

# 빠른 시작
## 설치
  1. `Plugins` 디렉터리를 UE 프로젝트 루트 디렉터리에 복사하세요.
  2. UE 프로젝트를 재시작하세요.

## UnLua 여행 시작
**주의**: UE 초보자라면, 더 자세한 [그림 설명서 버전](Docs/CN/Quickstart_For_UE_Newbie.md)을 사용하여 다음 단계를 진행하는 것을 추천합니다.
  1. 새 블루프린트를 생성한 후 열고, UnLua 도구 모음에서 `바인딩`을 선택하세요 (`Alt` 키를 동시에 눌러 2단계 경로를 자동 생성할 수 있습니다)
  2. 인터페이스의 `GetModule` 함수에 Lua 파일 경로를 입력하세요, 예: `GameModes.BP_MyGameMode`
  3. UnLua 도구 모음에서 `Lua 템플릿 파일 생성`을 선택하세요
  4. `Content/Script/GameModes/BP_MyGameMode.lua`를 열어 코드를 작성하세요

# 더 많은 예제
  * [01_HelloWorld](Content/Script/Tutorials/01_HelloWorld.lua) 빠른 시작 예제
  * [02_OverrideBlueprintEvents](Content/Script/Tutorials/02_OverrideBlueprintEvents.lua) 블루프린트 이벤트 재정의 (Overridden Functions)
  * [03_BindInputs](Content/Script/Tutorials/03_BindInputs.lua) 입력 이벤트 바인딩
  * [04_DynamicBinding](Content/Script/Tutorials/04_DynamicBinding.lua) 동적 바인딩
  * [05_BindDelegates](Content/Script/Tutorials/05_BindDelegates.lua) 대리자의 바인딩, 언바인딩, 트리거
  * [06_NativeContainers](Content/Script/Tutorials/06_NativeContainers.lua) 엔진 레벨 네이티브 컨테이너 접근
  * [07_CallLatentFunction](Content/Script/Tutorials/07_CallLatentFunction.lua) 코루틴에서 `Latent` 함수 호출
  * [08_CppCallLua](Content/Script/Tutorials/08_CppCallLua.lua) C++에서 Lua 호출
  * [09_StaticExport](Content/Script/Tutorials/09_StaticExport.lua) 커스텀 타입을 Lua로 정적 내보내기
  * [10_Replications](Content/Script/Tutorials/10_Replications.lua) 네트워크 복제 이벤트 재정의
  * [11_ReleaseUMG](Content/Script/Tutorials/11_ReleaseUMG.lua) UMG 관련 객체 해제
  * [12_CustomLoader](Content/Script/Tutorials/12_CustomLoader.lua) 커스텀 로더
  * [13_AnimNotify](Content/Script/Tutorials/AN_FootStep.lua) 애니메이션 알림

# 모범 사례 예제

[Lyra with UnLua](https://github.com/xuyanghuang-tencent/LyraWithUnLua) UE 공식 **Lyra 초보자 게임 패키지** 기반 완전한 예제 프로젝트, 현재 개발 중

# 문서

자주 사용하는 문서: [설정 옵션](Docs/CN/Settings.md) | [디버깅](Docs/CN/Debugging.md) | [인텔리센스](Docs/CN/IntelliSense.md) | [콘솔 명령](Docs/CN/ConsoleCommand.md) | [FAQ](Docs/CN/FAQ.md)

상세 소개:
* [프로그래밍 가이드](Docs/CN/UnLua_Programming_Guide.md): UnLua의 주요 기능과 프로그래밍 패턴 소개
* [플러그인과 모듈](Docs/CN/Plugins_And_Modules.md): Plugins 디렉터리 하위 플러그인 목록과 포함된 모듈 소개
* [기능 목록](Docs/CN/Features.md): 더 자세한 기능 목록
* [구현 원리](Docs/CN/How_To_Implement_Overriding.md): UnLua의 두 가지 재정의 메커니즘 소개
* [API](Docs/CN/API.md): 더 자세한 UnLua API 설명

# 기술 지원
- 공식 교류 QQ 그룹: 936285107
- 추천 VSCode 플러그인: [Lua Booster](https://marketplace.visualstudio.com/items?itemName=operali.lua-booster)