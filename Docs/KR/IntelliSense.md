# 지능형 제안
## 1. 지능형 제안 정보 생성
![지능형 제안 정보 생성](../Images/generate_intellisense.png)

UnLua 툴바를 열고 지능형 제안 익스포트를 클릭하면 `{UE프로젝트}/Plugins/UnLua/Intermediate`에 `IntelliSense` 디렉토리가 생성됩니다.

## 2. LuaIDE에 추가

![VSCode 워크스페이스](../Images/vscode_workspace.png)

VSCode를 예로 들어, 예제 프로젝트의 [TPSProject.code-workspace](../../TPSProject.code-workspace)를 참조하세요. 하나의 워크스페이스에 `Script`와 `IntelliSense`를 각각 추가하면, 지능형 제안 정보 생성 후 자동으로 새로고침되어 매번 수동으로 복사할 필요가 없습니다.

## 3. 완료

![UE 지능형 제안](../Images/ue_intellisense.png)

지능형 제안 정보가 작동하는지 확인하려면, 프로젝트의 임의 lua 파일에서 `UE.`를 입력하면 정상적으로 엔진의 모든 타입 제안이 표시되어야 합니다.

제안이 나타나지 않으면 IDE의 지능형 제안 플러그인이 존재하거나 활성화되어 있는지 확인해 보세요.