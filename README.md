# Muksi
<h1 align="center">Muksi</h1>
<p align="center">Unreal Engine 5 기반 턴제 카드 전투 게임</p>

<p align="center" style="line-height: 2;">
    <img src="https://img.shields.io/badge/Unreal%20Engine-0E1128?style=for-the-badge&logo=unrealengine&logoColor=white">
    <img src="https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=cplusplus&logoColor=white">
</p>

> 본 Repository는 포트폴리오 제출을 위해 실제 팀 프로젝트 Repository를 Fork한 저장소입니다.  
> Muksi는 현재 개발 진행 중인 프로젝트이며, 본 README에서는 제가 주로 담당한 **Battle System**을 중심으로 소개합니다.

<br/>

## 시연

> Battle System 시연 영상

<!-- 영상이 준비되면 아래 형식으로 추가 -->
<!--
[![Muksi Battle 소개영상](썸네일 주소)](영상 주소)
-->

<!-- 영상이 없다면 Battle 스크린샷 사용 -->
<p align="center">
    <img src="./page_image/Battle.png" width="700"/>
</p>

<br/>

## 목차

- [프로젝트 소개](#프로젝트-소개)
- [담당 역할](#담당-역할)
- [Battle Flow](#battle-flow)
- [주요 구현](#주요-구현)
- [시스템 구조도](#시스템-구조도)
- [Troubleshooting](#troubleshooting)
- [개발 회고](#개발-회고)

<br/>

## 프로젝트 소개

Muksi는 무협 세계관을 기반으로 한 턴제 카드 전투 게임입니다.

플레이어와 적은 각 Exchange에서 카드를 선택하고 타겟을 지정하며,
선택된 행동은 전투 순서에 따라 실행됩니다.

현재 프로젝트는 개발 진행 중이며,
제가 주로 담당한 Battle 콘텐츠는 카드 선택부터 타겟 지정,
행동 실행 및 전투 연출까지 이어지는 전투 흐름을 중심으로 구현되어 있습니다.

| 항목 | 내용 |
| --- | --- |
| 장르 | Turn-Based Card Battle |
| 엔진 | Unreal Engine 5.7.4 |
| 언어 | C++ |
| 개발 형태 | Team Project |
| 담당 영역 | Battle System / Battle UI |
| 개발 기간 | YYYY.MM ~ 진행 중 |

<br/>

## 담당 역할

**Battle System / Battle UI Programming**

본 프로젝트에서 Battle System을 중심으로 아래 기능을 개발했습니다.

- Battle Phase 기반 전투 진행 시스템 구현 및 개선
- 플레이어 / 적 카드 선택 및 Battle Card 관리 시스템
- 카드 선택 → 타겟 지정 → 행동 실행으로 이어지는 전투 흐름 구현
- Enemy AI 카드 선택 및 전투 흐름 연동
- Battle UI 및 카드 Drag & Drop / Slot 상호작용 구현
- Animation Montage / Notify 기반 전투 행동 및 연출 연동
- 제한 시간 초과 시 Panic 행동 처리 시스템
- Battle 시스템 구조 분석 및 클래스별 책임 분리

> 팀 프로젝트이므로 아래의 `주요 구현`에서는
> 제가 직접 개발하거나 주도적으로 개선한 Battle System을 중심으로 설명합니다.

<br/>

## Battle Flow

Battle은 Phase를 기준으로 순차적으로 진행됩니다.

```text
BattleStart
    ↓
RoundStart
    ↓
ExchangeStart
    ↓
CardSelect
    ↓
ExchangeEnd
    ↓
BattleActionSequenceStart
    ↓
BattleActionSequenceEnd
    ↓
RoundEnd
```

`BattleManager`가 현재 Battle Phase를 관리하며,
각 Phase의 변화에 따라 카드 선택, 타겟 지정, UI 연출,
Battle Action 실행이 순차적으로 진행되도록 구성했습니다.

<!-- Figma 또는 직접 만든 Flow 이미지가 있다면 추가 -->
<p align="center">
    <!-- <img src="./page_image/BattleFlow.png" width="750"/> -->
</p>

<br/>

## 주요 구현

### 1. Battle Phase System

Battle의 진행 상태를 Phase 단위로 구분하고,
`BattleManager`를 중심으로 전투 흐름을 관리하도록 구성했습니다.

각 Phase가 변경되면 해당 단계에서 필요한 게임 로직과 UI가 실행되며,
카드 선택부터 Battle Action 실행까지 하나의 흐름으로 이어집니다.

- Battle / Round / Exchange 단위의 전투 상태 관리
- CardSelect 단계에서 플레이어 입력 처리
- Exchange 종료 후 Battle Action Sequence 실행
- Phase 변경에 따른 Battle UI 연동

#### 핵심 코드

```cpp
// 실제 BattleManager 코드에서
// Phase 변경과 관련된 핵심 부분을 추후 추가
```

**관련 코드**
- `Source/.../BattleManager.cpp`
- `Source/.../BattleManager.h`

<br/>

### 2. Battle Card System

캐릭터가 보유한 카드와 선택된 카드를
Battle UI와 분리하여 관리할 수 있도록 카드 시스템을 구성했습니다.

`BattleCardComponent`에서 캐릭터의 카드 상태를 관리하고,
UI에서는 카드의 표시와 상호작용을 담당하도록 역할을 분리했습니다.

- Current Hand 관리
- 선택된 카드 Commit / Return 처리
- Exchange별 카드 선택 관리
- 사용된 카드 소비 및 손패 재구성
- 카드 Instance 단위 관리

#### 핵심 코드

```cpp
// BattleCardComponent의 CommitCard,
// ReturnCommittedCard 등의 핵심 코드 추가
```

**관련 코드**
- `Source/.../BattleCardComponent.cpp`
- `Source/.../BattleCardComponent.h`
- `Source/.../BattleCardManager.cpp`

<br/>

### 3. Card Interaction & Battle UI

플레이어가 카드를 Drag하여 Exchange Slot에 배치하고,
선택한 카드에 따라 Targeting 단계로 이어지도록 UI를 구현했습니다.

- 카드 Drag & Drop
- Exchange Slot Overlap 판정
- 카드 Equip / Unequip
- 카드 선택 후 Targeting 연동
- Phase에 따른 카드 상호작용 활성 / 비활성
- Enemy 카드 선택 및 Reveal 연출

#### 핵심 코드

```cpp
// UWidget_BattleCardBase::StopDragging()
// UWidget_CardEquipSlot::EquipCard()
// 등 핵심 코드 추가
```

**관련 코드**
- `Source/.../Widget_BattleCardBase.cpp`
- `Source/.../HandWidget.cpp`
- `Source/.../Widget_CardEquipSlot.cpp`
- `Source/.../ExchangeSlotPanelWidget.cpp`
- `Source/.../Widget_BattleMainScreen.cpp`

<br/>

### 4. Battle Action & Animation System

선택된 카드 행동을 Battle Action으로 구성하고,
행동 실행 과정에서 Animation Montage와 Notify를 이용해
실제 공격 및 전투 연출이 필요한 시점에 실행되도록 연결했습니다.

- Battle Action 순서 구성
- 캐릭터 행동 실행
- Animation Montage 재생
- Anim Notify를 통한 전투 이벤트 전달
- 카메라 및 전투 연출 연결

#### 핵심 코드

```cpp
// StartCurrentBattleAction()
// Notify 기반 Execution 처리 등
// 실제 핵심 코드 추가
```

**관련 코드**
- `Source/.../BattleSequenceManager.cpp`
- `Source/.../BattleExecution...`
- `Source/.../MuksiBattleExecutionAnimNotify.cpp`

<br/>

### 5. Timeout / Panic System

카드 선택 제한 시간이 종료되었을 때
행동이 선택되지 않은 캐릭터가 자동으로 Panic 행동을 수행하도록 구현했습니다.

단순한 랜덤 카드 선택에서 시작했지만,
캐릭터별 Panic 데이터와 Strategy를 통해
각 캐릭터의 특성에 맞는 행동을 결정할 수 있도록 확장했습니다.

- CardSelect 제한 시간 처리
- 행동 미선택 여부 확인
- Panic Card 결정
- 기존 선택 카드 반환 및 교체
- Panic Targeting Strategy 연동

#### 핵심 코드

```cpp
// ResolvePlayerPanicOnTimeout()
// ApplyPlayerPanicTimeoutResult()
// 등의 핵심 코드 추가
```

**관련 코드**
- `Source/.../BattleCardManager.cpp`
- `Source/.../Widget_BattleMainScreen.cpp`
- `Source/.../PanicStrategyBase.cpp`

<br/>

## 시스템 구조도

> 아래 구조도는 Muksi의 Battle System을 중심으로 한 구조를 나타냅니다.

<!-- 추후 Figma 또는 Battle 구조 이미지 삽입 -->
<p align="center">
    <!-- <img src="./page_image/BattleSystemStructure.png" width="750"/> -->
</p>

```text
BattleManager
   │
   ├─ Battle Phase
   │
   ├─ Battle Card Manager
   │     └─ BattleCardComponent
   │
   ├─ Battle Targeting Manager
   │
   ├─ Battle Action / Execution
   │
   └─ Battle UI
         ├─ BattleMainScreen
         ├─ HandWidget
         └─ ExchangeSlotPanel
```

- `BattleManager`가 전체 Battle Phase와 전투 흐름을 관리합니다.
- 카드 데이터와 상태는 캐릭터의 `BattleCardComponent`에서 관리합니다.
- Battle UI는 Phase와 Battle 상태에 따라 카드 및 전투 연출을 표시합니다.
- 선택된 행동은 Targeting을 거쳐 Battle Action / Execution 단계에서 실행됩니다.

<br/>

## Troubleshooting

### Battle System 통합 과정에서 책임과 호출 흐름이 중첩된 문제

**문제**

Battle Phase System과 선행 개발되어 있던 Execution System을
하나의 전투 흐름으로 통합하는 과정에서 담당 영역이 겹치기 시작했고,
전투 단계와 UI, Execution의 책임이 여러 클래스에 중첩되는 문제가 발생했습니다.

이로 인해 기능을 수정하거나 추가할 때 여러 코드를 함께 확인해야 했고,
클래스별 역할이 불명확해 개발 진척이 느려졌습니다.

**해결**

각 Battle Phase의 시작과 종료,
이후 Execution으로 이어지는 함수 호출 흐름을 코드에서 직접 추적하고
Figma에 순서도로 정리했습니다.

또한 변수와 함수에서 서로 다르게 사용되던 용어를 정리하고,
팀원과 각 클래스가 맡아야 할 책임을 논의하여
전투 진행, 행동 실행, UI 표시와 연출의 역할을 분리했습니다.

**결과**

이후 새로운 Battle 기능을 추가할 때 Figma의 흐름을 기준으로
어느 클래스에서 구현해야 하는지,
어떤 함수로 정보를 받고 다음 단계에 무엇을 전달해야 하는지를
팀원과 빠르게 논의할 수 있게 되었습니다.

<br/>

## 개발 회고

Muksi의 Battle System을 개발하면서
기능 자체를 구현하는 것뿐 아니라 여러 시스템이 연결될수록
각 클래스의 책임과 데이터 흐름을 명확하게 관리하는 것이 중요하다는 점을 경험했습니다.

초기에는 새로운 기능을 빠르게 추가하는 데 집중하면서
UI, Battle 상태, 카드 데이터 사이의 책임이 겹치는 부분이 발생했습니다.
이후 기존 코드의 호출 흐름과 데이터 변경 지점을 다시 확인하고,
각 시스템의 역할을 분리하는 방향으로 구조를 지속적으로 개선했습니다.

이 경험을 통해 기능의 현재 동작뿐 아니라
이후의 변경과 확장, 다른 시스템과의 연결까지 고려하여
구조를 설계하는 것이 중요하다는 점을 배웠습니다.
