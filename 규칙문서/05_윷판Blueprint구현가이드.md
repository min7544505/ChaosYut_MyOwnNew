# 윷판 Blueprint 구현 가이드

## 개요
29개 정점을 가진 윷판 시스템을 Blueprint로 구현하는 단계별 가이드

---

## Step 1: Enum 생성 - E_RoadType

**경로:** Content/Data/E_RoadType

| 값 | 설명 |
|----|------|
| Normal | 일반 정점 |
| Branch | 지름길 입구 (C2, C3) |
| Merge | 합류점 (C1, C4) |
| Center | 교차로 (방) |
| ItemSpot | 아이템 지점 |

**생성 방법:**
1. Content Browser → 우클릭 → Blueprints → Enumeration
2. 이름: E_RoadType
3. 위 값들 추가

---

## Step 2: Struct 생성 - ST_RoadData

**경로:** Content/Data/ST_RoadData

| 필드명 | 타입 | 설명 |
|--------|------|------|
| RoadID | Name | 정점 고유 ID |
| RoadType | E_RoadType | 정점 타입 |
| PositionX | Float | X 좌표 |
| PositionY | Float | Y 좌표 |
| PositionZ | Float | Z 좌표 |
| NextRoads | String | 다음 정점들 (파이프로 구분) |
| PrevRoads | String | 이전 정점들 (파이프로 구분) |
| IsCorner | Boolean | 꼭짓점 여부 |
| IsDiagonal | Boolean | 대각선 경로 여부 |

**생성 방법:**
1. Content Browser → 우클릭 → Blueprints → Structure
2. 이름: ST_RoadData
3. 위 필드들 추가

---

## Step 3: DataTable 임포트 - DT_BoardLayout

**경로:** Content/Data/DT_BoardLayout

**방법 1: CSV 임포트**
1. Content/Data/DT_BoardLayout.csv 파일이 이미 생성됨
2. Content Browser → Import → CSV 선택
3. Row Type: ST_RoadData 선택

**방법 2: 직접 생성**
1. Content Browser → 우클릭 → Miscellaneous → Data Table
2. Row Structure: ST_RoadData
3. 29개 행 수동 입력

---

## Step 4: BP_RoadBase 구현

### 4.1 컴포넌트 구성
```
BP_RoadBase (Actor)
├── DefaultSceneRoot (Scene)
├── RoadMesh (Static Mesh) - 정점 시각화
└── DetectionBox (Box Collision) - 말 감지
```

### 4.2 변수
| 변수명 | 타입 | 기본값 | 설명 |
|--------|------|--------|------|
| RoadID | Name | None | 정점 ID |
| RoadType | E_RoadType | Normal | 정점 타입 |
| NextRoads | Array<BP_RoadBase> | [] | 전진 시 다음 정점들 |
| PrevRoads | Array<BP_RoadBase> | [] | 후진 시 이전 정점들 |
| CurrentPieces | Array<BP_YutPiece> | [] | 현재 위치한 말들 |
| IsCorner | Boolean | false | 꼭짓점 여부 |
| IsDiagonal | Boolean | false | 대각선 경로 여부 |

### 4.3 함수

#### InitializeRoad(RoadData: ST_RoadData)
```
- RoadID = RoadData.RoadID
- RoadType = RoadData.RoadType
- SetActorLocation(RoadData.Position)
- IsCorner = RoadData.IsCorner
- IsDiagonal = RoadData.IsDiagonal
```

#### GetNextRoad(bPreferDiagonal: Boolean) → BP_RoadBase
```
- NextRoads가 1개면 그것 반환
- NextRoads가 2개 이상이면:
  - bPreferDiagonal이면 대각선 경로 반환
  - 아니면 외곽 경로 반환
```

#### GetPrevRoad(bPreferDiagonal: Boolean) → BP_RoadBase
```
- PrevRoads가 1개면 그것 반환
- PrevRoads가 2개 이상이면:
  - bPreferDiagonal이면 대각선 경로 반환
  - 아니면 외곽 경로 반환
```

#### OnPieceEnter(Piece: BP_YutPiece)
```
- CurrentPieces에 Piece 추가
- 아이템 지점이면 아이템 획득 이벤트 발생
```

#### OnPieceExit(Piece: BP_YutPiece)
```
- CurrentPieces에서 Piece 제거
```

---

## Step 5: BP_BranchRoad 구현 (지름길 입구)

**부모 클래스:** BP_RoadBase

### 추가 변수
| 변수명 | 타입 | 설명 |
|--------|------|------|
| OuterNext | BP_RoadBase | 외곽 다음 정점 |
| DiagonalNext | BP_RoadBase | 대각선 다음 정점 |

### Override: GetNextRoad
```
IF 플레이어가 이 정점에 딱 멈춤 THEN
    선택 UI 표시 (외곽 vs 대각선)
    선택 결과 반환
ELSE
    OuterNext 반환 (지나가는 경우)
```

**적용 대상:** C2, C3

---

## Step 6: BP_MergeRoad 구현 (합류점)

**부모 클래스:** BP_RoadBase

### 추가 변수
| 변수명 | 타입 | 설명 |
|--------|------|------|
| OuterPrev | BP_RoadBase | 외곽 이전 정점 |
| DiagonalPrev | BP_RoadBase | 대각선 이전 정점 |

### Override: GetPrevRoad
```
IF 백도 상황 THEN
    선택 UI 표시 (외곽 vs 대각선)
    선택 결과 반환
ELSE
    기본 동작
```

**적용 대상:** C1, C4

---

## Step 7: BP_CenterRoad 구현 (교차로)

**부모 클래스:** BP_RoadBase

### 추가 변수
| 변수명 | 타입 | 설명 |
|--------|------|------|
| ExitToC4 | BP_RoadBase | C4 방향 출구 (D_E) |
| ExitToC1 | BP_RoadBase | C1 방향 출구 (D_F) |
| EntryFromC2 | BP_RoadBase | C2 방향 입구 (D_D) |
| EntryFromC3 | BP_RoadBase | C3 방향 입구 (D_C) |

### Override: GetNextRoad
```
선택 UI 표시 (C4 방향 vs C1 방향)
선택 결과 반환
```

### Override: GetPrevRoad
```
선택 UI 표시 (C2 방향 vs C3 방향)
선택 결과 반환
```

**적용 대상:** CENTER (방)

---

## Step 8: BP_YutBoard 구현

### 8.1 변수
| 변수명 | 타입 | 설명 |
|--------|------|------|
| BoardLayoutTable | DataTable | DT_BoardLayout 참조 |
| AllRoads | Map<Name, BP_RoadBase> | 모든 정점 맵 |
| StartRoad | BP_RoadBase | 시작점 (C1) |

### 8.2 함수

#### BeginPlay / InitializeBoard
```
1. BoardLayoutTable에서 모든 행 가져오기
2. 각 행에 대해:
   a. RoadType에 따라 적절한 클래스 스폰
      - Normal → BP_RoadBase
      - Branch → BP_BranchRoad
      - Merge → BP_MergeRoad
      - Center → BP_CenterRoad
   b. InitializeRoad() 호출
   c. AllRoads에 추가
3. 연결 설정 (SetupConnections)
4. StartRoad = AllRoads["C1"]
```

#### SetupConnections
```
각 Road에 대해:
1. NextRoads 문자열 파싱 ("|"로 분리)
2. 각 ID에 해당하는 Road를 AllRoads에서 찾아 연결
3. PrevRoads도 동일하게 처리
```

#### GetRoadByID(ID: Name) → BP_RoadBase
```
return AllRoads[ID]
```

#### CalculatePath(From: BP_RoadBase, Steps: int, bBackward: bool) → Array<BP_RoadBase>
```
Path = []
Current = From

FOR i = 0 TO Steps-1:
    IF bBackward THEN
        Next = Current.GetPrevRoad()
    ELSE
        Next = Current.GetNextRoad()

    IF Next == None THEN (골인)
        BREAK

    Path.Add(Next)
    Current = Next

return Path
```

---

## Step 9: 테스트

### 9.1 레벨 설정
1. BasicYutLevel 열기
2. BP_YutBoard를 레벨에 배치
3. BoardLayoutTable에 DT_BoardLayout 할당

### 9.2 확인 사항
- [ ] PIE 실행 시 29개 정점 생성
- [ ] 각 정점이 올바른 위치에 배치
- [ ] 정점 간 연결이 올바른지 확인

### 9.3 테스트 케이스
| 테스트 | 예상 결과 |
|--------|----------|
| C1에서 도(1칸) | O_L 도착 |
| C1에서 모(5칸) | C2 도착 |
| C2에서 대각선 선택 후 개(2칸) | D_D 도착 |
| CENTER에서 전진 | D_E 또는 D_F 선택 |
| C1에서 백도 | O_P 또는 D_H 선택 |

---

## 정점 좌표 참고 (단위: cm)

```
        C3(-500,500)───a───b───c───d───C2(500,500)
              │ ╲                   ╱ │
              e    A             B    f
              │      ╲         ╱      │
              g         C   D         h
              │       CENTER          │
              i         E   F         j
              │      ╱         ╲      │
              k    G             H    l
              │ ╱                   ╲ │
        C4(-500,-500)──m───n───o───p──C1(500,-500)
```

| ID | X | Y | ID | X | Y |
|----|---|---|----|---|---|
| C1 | 500 | -500 | CENTER | 0 | 0 |
| C2 | 500 | 500 | D_B | 350 | 350 |
| C3 | -500 | 500 | D_D | 175 | 175 |
| C4 | -500 | -500 | D_A | -350 | 350 |
| O_L | 500 | -300 | D_C | -175 | 175 |
| O_J | 500 | -100 | D_E | -175 | -175 |
| O_H | 500 | 100 | D_G | -350 | -350 |
| O_F | 500 | 300 | D_F | 175 | -175 |
| ... | ... | ... | D_H | 350 | -350 |
