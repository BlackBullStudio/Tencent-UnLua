UE가 물리 충돌에서 기본적으로 제공하는 객체 타입 외에도, `프로젝트 설정 -> 엔진 -> 충돌`에서 프로젝트 사용자 정의 충돌 구성을 추가할 수 있습니다. 구체적인 구성 작업은 [공식 문서](https://docs.unrealengine.com/4.27/zh-CN/InteractiveExperiences/Physics/Collision/HowTo/AddCustomCollisionType/)를 참조하세요.

Lua에서 사용할 때는 **구성 이름**을 통해 직접 접근할 수 있습니다. 예를 들어:

충돌 감지 채널:
`UE.ECollisionChannel.Pawn`

객체 타입 쿼리:
`UE.EObjectTypeQuery.Player`

추적 타입 쿼리:
`UE.ETraceTypeQuery.Weapon`