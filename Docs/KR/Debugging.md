# 디버깅
## LuaPanda / LuaHelper를 사용한 디버깅

### 1. 준비 작업
1. VSCode 마켓플레이스에서 [LuaPanda](https://marketplace.visualstudio.com/items?itemName=stuartwang.luapanda) / [LuaHelper](https://marketplace.visualstudio.com/items?itemName=yinfei.luahelper) 설치
2. [LuaPanda 공식 저장소](https://github.com/Tencent/LuaPanda/tree/master/Debugger)에서 `LuaPanda.lua`를 가져와 `{UE프로젝트}/Content/Script` 디렉토리에 배치
3. Lua 코드에 `require("LuaPanda").start("127.0.0.1",8818)` 추가

### 2. 디버깅 시작
VSCode 환경의 경우 각 플러그인의 구성 문서를 참조하세요. 여기서는 자세히 설명하지 않습니다.
- [LuaPanda](https://github.com/Tencent/LuaPanda)
- [LuaHelper](https://github.com/Tencent/LuaHelper)

참고: 디버거는 `luasocket`에 의존하며, UnLua는 확장 플러그인을 통해 이미 통합했습니다. 연결할 수 없는 경우 `{UE프로젝트}/Plugins/UnLuaExtensions` 디렉토리가 존재하는지 확인하세요.

## LuaBooster를 사용한 디버깅

TODO: