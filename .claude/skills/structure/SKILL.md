# /structure Skill

프로젝트 구조 빠른 파악

## Usage
```
/structure            - 프로젝트 구조 요약
/structure --detailed - 상세 구조
/structure --content  - Content 폴더만
```

## Examples
```
/structure
/structure --detailed
/structure --content
```

## Project Path
`E:\UnrealProject\ChaosYut_New`

---

## Output Format

### 기본 구조 (/structure)
```
## ChaosYut_New 프로젝트 구조

### 프로젝트 정보
- 엔진: Unreal Engine 5.6
- 경로: E:\UnrealProject\ChaosYut_New

### Content 폴더
Content/
├── BluePrint/          (게임 로직)
├── Data/               (데이터 에셋)
├── UI/                 (게임 모드, 컨트롤러, UI)
├── Material/           (머티리얼)
├── Mesh/               (메시)
├── Design/             (디자인 에셋)
└── Fab/                (외부 에셋)

### 주요 Blueprint
| Blueprint | 크기 | 역할 |
|-----------|------|------|
| WBP_YutButton | XX KB | UI 위젯 |
| BP_YutBoard | XX KB | 보드 관리 |
| BP_YutPiece | XX KB | 게임 말 |
| PC_Yut | XX KB | 게임 컨트롤러 |
| BP_RoadBase | XX KB | 보드 위치 |

### 레벨
- BasicYutLevel.umap (메인 레벨)
```

---

### 상세 구조 (/structure --detailed)
```
## ChaosYut_New 상세 구조

### 폴더별 파일 수
| 폴더 | 파일 수 | 총 크기 |
|------|---------|---------|
| BluePrint | X개 | XXX KB |
| Data | X개 | XX KB |
| UI | X개 | XXX KB |
| Material | X개 | XX KB |
| Mesh | X개 | XX KB |

### Blueprint 상세
[각 Blueprint별 변수, 함수, 이벤트 수]

### 데이터 에셋
[DataTable, Structure, Enum 목록]
```

---

## Directory Structure

### 전체 구조
```
ChaosYut_New/
├── .claude/                  (Claude 설정)
│   ├── agents/               (에이전트)
│   ├── skills/               (스킬)
│   └── settings.local.json
│
├── Content/                  (게임 에셋)
│   ├── BluePrint/
│   ├── Data/
│   ├── UI/
│   ├── Material/
│   ├── Mesh/
│   ├── Design/
│   └── Fab/
│
├── Config/                   (설정)
├── Saved/                    (저장)
├── Intermediate/             (빌드 캐시)
├── 규칙문서/                 (기획 문서)
│
└── ChaosYut_New.uproject
```

---

## Commands

```bash
# 전체 구조 (깊이 2)
tree "E:/UnrealProject/ChaosYut_New" -L 2

# Content 구조
tree "E:/UnrealProject/ChaosYut_New/Content" -L 2

# Blueprint 목록과 크기
ls -la "E:/UnrealProject/ChaosYut_New/Content/BluePrint/"
ls -la "E:/UnrealProject/ChaosYut_New/Content/UI/"
ls -la "E:/UnrealProject/ChaosYut_New/Content/Data/"

# 총 파일 수
find "E:/UnrealProject/ChaosYut_New/Content" -name "*.uasset" | wc -l
```
