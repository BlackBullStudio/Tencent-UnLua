# 기능 목록

## 개요
* 정적 바인딩과 동적 바인딩
* 에디터 환경에서 서버/클라이언트 시뮬레이션
* 블루프린트 템플릿 생성 및 익스포트
* 수동 정적 익스포트
* 구조체 접근 최적화
* `UE` 네임스페이스를 통한 C++ 타입 접근

## 플랫폼 지원
* 실행 플랫폼: Windows / Android / iOS / Linux / OSX
* 엔진 버전: Unreal Engine 4.17.x - Unreal Engine 5.x

## Lua로 대체 구현 지원
* 블루프린트의 모든 `BlueprintImplementableEvent` 또는 `BlueprintNativeEvent`로 표시된 "블루프린트 이벤트"
* 모든 네트워크 복제 이벤트 알림(Replication Notify)
* 모든 애니메이션 이벤트 알림(Animation Notify)
* 모든 입력 이벤트 알림(Input Event)

## Lua에서 블루프린트 호출
* 모든 `UCLASS`, `UPROPERTY`, `UFUNCTION`, `USTRUCT`, `UENUM` 접근 지원
* 이미 오버라이드된 원본 블루프린트 로직 접근 지원
* `BlueprintCallable` 또는 `Exec`으로 표시된 모든 `UFUNCTION`의 기본 매개변수 지원
* 코루틴에서 `Latent` 함수 호출 지원

## Lua에서 C++ 호출
* 단일 캐스트 델리게이트 바인딩, 언바인딩, 트리거
* 멀티 캐스트 델리게이트 바인딩, 언바인딩, 정리, 브로드캐스트
* `UENUM` 접근
* [사용자 정의 충돌 열거형](CollisionEnum.md) 지원
* 멤버 변수
* 멤버 함수
* 정적 멤버 변수
* 정적 멤버 함수
* 전역 함수

## C++에서 Lua 호출
* 전역 함수
* 전역 테이블 내의 함수

## 기타 도구
* [사용자 정의 Lua 템플릿 생성](CustomTemplate.md) 지원