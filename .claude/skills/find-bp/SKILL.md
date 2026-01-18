# /find-bp Skill

Blueprint 에셋 빠른 검색

## Usage
```
/find-bp              - 모든 Blueprint 목록
/find-bp [검색어]     - 검색어로 필터링
/find-bp --size       - 크기순 정렬
/find-bp --recent     - 최근 수정순 정렬
```

## Examples
```
/find-bp
/find-bp Yut
/find-bp Road
/find-bp --size
/find-bp --recent
```

## Project Path
`E:\UnrealProject\ChaosYut_New\Content`

---

## Blueprint Categories

### Game Logic (게임 로직)
| Blueprint | 설명 |
|-----------|------|
| `PC_Yut` | 게임 컨트롤러 |
| `GM_Yut` | 게임 모드 |

### Board (보드)
| Blueprint | 설명 |
|-----------|------|
| `BP_YutBoard` | 보드 관리자 |
| `BP_RoadBase` | 기본 길 |
| `BP_BranchRoad` | 갈림길 |
| `BP_CenterRoad` | 중앙 길 |
| `BP_MergedRoad` | 합류 길 |

### Pieces (말)
| Blueprint | 설명 |
|-----------|------|
| `BP_YutPiece` | 게임 말 |

### UI (유저 인터페이스)
| Blueprint | 설명 |
|-----------|------|
| `WBP_YutButton` | 윷 버튼 위젯 |
| `BP_PlayerStartView` | 초기 카메라 |

### Data (데이터)
| Blueprint | 설명 |
|-----------|------|
| `DT_YutData` | 윷 데이터 테이블 |
| `ST_YutData` | 윷 구조체 |
| `E_Yut` | 윷 열거형 |

---

## Output Format

### 기본 목록
```
## Blueprint 목록

| 파일명 | 경로 | 크기 | 수정일 |
|--------|------|------|--------|
| BP_YutBoard.uasset | Content/BluePrint | XX KB | YYYY-MM-DD |
| BP_YutPiece.uasset | Content/BluePrint | XX KB | YYYY-MM-DD |
| ... | ... | ... | ... |
```

### 크기순 정렬 (--size)
```
## Blueprint (크기순)

1. WBP_YutButton.uasset (XXX KB)
2. BP_YutBoard.uasset (XXX KB)
3. BP_YutPiece.uasset (XX KB)
...
```

---

## Search Commands

```bash
# 모든 Blueprint 검색
find "E:/UnrealProject/ChaosYut_New/Content" -name "*.uasset" | sort

# 특정 이름 검색
find "E:/UnrealProject/ChaosYut_New/Content" -name "*Yut*.uasset"

# 크기순 정렬
find "E:/UnrealProject/ChaosYut_New/Content" -name "*.uasset" -exec ls -la {} \; | sort -k5 -rn

# 최근 수정순
find "E:/UnrealProject/ChaosYut_New/Content" -name "*.uasset" -exec ls -la {} \; | sort -k6,7 -r
```
