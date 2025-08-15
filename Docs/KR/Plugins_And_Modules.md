# 플러그인과 모듈 설명

## UnLua

핵심 기능 플러그인으로, UnLua를 설치하여 사용할 때 최소한 프로젝트 Plugin 디렉토리에 이것이 있어야 합니다.

모듈 목록:
* UnLua 주요 런타임 모듈
* UnLuaEditor 에디터 모듈, Lua 템플릿 생성/지능형 제안 익스포트/콘솔 명령어 등 기능 제공
* UnLuaDefaultParamCollector 인코딩 모듈, UFUNCTION의 기본 매개변수 수집

## UnLuaExtensions

UnLua 확장 예제 플러그인으로, 일부 서드파티 라이브러리를 통합할 때 그 구현을 참조할 수 있습니다.

모듈 목록:
* [LuaSocket](https://github.com/lunarmodules/luasocket) 일부 순수 Lua 디버거가 이 라이브러리를 필요로 할 수 있습니다

## UnLuaTestSuite

자동화 테스트 플러그인으로, UnLua가 제공하는 API의 표준 테스트와 일부 Issue에 해당하는 회귀 테스트를 포함합니다.