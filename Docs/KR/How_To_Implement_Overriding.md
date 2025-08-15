# 개요
제로 글루 코드로 다양한 블루프린트와 알림 이벤트를 대체하는 것은 `UnLua`의 주요 기능 중 하나입니다. 본 문서는 이 메커니즘의 원리와 해당하는 두 가지 방안을 주로 소개합니다.

---

# Thunk 함수 대체

## UFunction의 Thunk 함수

![THUNK_FUNC](../Images/ufunction_thunk_func.png)

여기서 `Func`가 소위 **thunk** 함수입니다.

## UFunction의 호출

![UOBJECT_PROCESSEVENT](../Images/uobject_processevent.png)

![UFUNCTION_INVOKE](../Images/ufunction_invoke.png)

Lua를 호출할 수 있는 함수가 있다고 가정하면, 이를 사용하여 엔진의 기본 thunk 함수를 대체할 수 있으며, 이는 UFunction의 구현을 오버라이드하는 것과 같습니다.

---

# Opcode 주입

또한 네이티브가 아닌 UFunction을 호출하는 다른 방법이 있습니다:

![UOBJECT_CALLFUNCTION](../Images/uobject_callfunction.png)

이런 경우 thunk 함수를 대체하는 것은 효과가 없지만, UFunction에 `특별한 Opcode`를 주입하여 오버라이드를 구현할 수 있습니다.

## 새로운 Opcode 정의

![NEW_OPCODE](../Images/new_opcode.png)

## 새로운 Opcode 등록

![OPCODE_REGISTRAR](../Images/opcode_registrar.png)

## UFunction 주입

![OPCODE_INJECTION](../Images/opcode_injection.png)

![CUSTOMIZED_THUNK_FUNC](../Images/customized_thunk_func.png)