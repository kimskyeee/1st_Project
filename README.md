# 목표를 포착했다
목표를 포착했다는 FPS게임 오버워치 2의 플레이어 **솔저 76**를 언리얼 엔진 Blueprint으로만 구현한 프로젝트입니다.

🎥 아래의 사진을 클릭시 전체 게임 플레이 영상을 보실 수 있습니다.

[![Video Label](http://img.youtube.com/vi/HfFOyyPGaP8/0.jpg)](https://youtu.be/HfFOyyPGaP8)

<br>

## 사용 기술
- Unreal Engine 5.5
- Blueprint
- Rider

<br>

## 프로젝트 개요
| 항목 | 내용 |
| --- | --- |
| **장르** | 히어로 슈팅 FPS 게임 |
| **플랫폼** | PC |
| **맵 컨셉** | 일정 시간안에 목적지 까지 화물을 운반하면 승리합니다. |
| **개발 내용** | 플레이어, 스킬(궁극기, 회복기 등), UI |

<br>

## 구현 상세 설명
각 기능의 더 자세한 설명은 [여기](https://kimskye.notion.site/1cfb7f13b7a6800b863de8b3479e32b7?pvs=74)서 확인하실 수 있습니다.

---

## 1. 궁극기(Q) - 오토 타겟팅
플레이어가 스킬을 발동하면, 시야 내 가장 가까운 적을 탐지하여 자동으로 조준 방향을 보정하는 오토 타겟팅 기능을 구현했습니다.

![궁](https://github.com/user-attachments/assets/e47679a2-3617-4e2c-bbfd-6684718ca3ab)

<img width="1044" height="334" alt="image" src="https://github.com/user-attachments/assets/648eeec5-800b-420e-bcee-8c2c574e81e0" />


- 타겟 탐지 함수 `FindNearest()`에서
  
  - `GetAllActorsWithTag("Enemy")`로 적을 수집하고 `GetDistanceTo()`를 통해 플레이어와 거리 계산합니다.
  - `ForEachLoop` 내에서 **최소 거리 비교** 로직을 적용한 뒤 최종적으로 `NearestActor`를 반환합니다.
      
- 자동 조준 `TacticalVisor()` 함수에서
  
  - `FindNearest()`를 호출해 반환된 `NearestActor`를 `CurrentTarget`에 저장합니다.
  - `FindLookAtRotation(SelfLocation, TargetLocation)`으로 회전을 계산하고, `SetControlRotation()`을 통해 카메라 시점을 적 방향으로 조정합니다.

<br>

---

## 2. 회복기(E) - 생체장(Biotic Field)
플레이어가 설치형 장비를 바닥에 배치하면, 일정 범위 내 아군 또는 본인이 지속적으로 체력을 회복하는 스킬을 구현했습니다.

![회복](https://github.com/user-attachments/assets/67011aa6-acb7-4e59-917d-4387cfa8c70b)

- `SpawnActor`로 플레이어의 위치에 반경 4.5m의 `BioticFieldActor` 배치하고, 5초 후 자동 파괴되게 Timer를 설정합니다.
- `Sphere Collision`범위에 `Overlap` 감지한 후 회복 대상 판단해 진입한 액터가 `HealthSystemComponent` 를 가지고 있다면 회복 대상임을 확정합니다.
- `TimerHandle`을 사용해 1초 간격으로 `HealthSystemComponent` 의 `IncreaseHealth()` 호출해 40씩 체력을 회복합니다.

<br>

---

<br>

## 트러블 슈팅
- **오토 타겟팅 버벅임 문제**  해결했습니다.
- 프로젝트 관련 트러블 슈팅은 [여기](https://www.notion.so/kimskye/1cfb7f13b7a6800b863de8b3479e32b7?source=copy_link#20bb7f13b7a680f0ab58cd0cb7356ce7)에서 확인하실 수 있습니다.
