# /compare Skill

원본 프로젝트(ChaosYut_DM)와 현재 프로젝트(ChaosYut_New) 비교

## Usage
```
/compare              - 전체 프로젝트 비교
/compare [blueprint]  - 특정 Blueprint 비교
/compare all          - 모든 Blueprint 상세 비교
```

## Examples
```
/compare
/compare BP_YutPiece
/compare BP_YutBoard
/compare all
```

## Project Paths
- **원본**: `E:\UnrealProject\ChaosYut_DM`
- **현재**: `E:\UnrealProject\ChaosYut_New`

## Comparison Output

### 전체 비교 (/compare)
```markdown
## 프로젝트 비교: ChaosYut_DM vs ChaosYut_New

### Blueprint 크기 비교
| Blueprint | 원본 | 현재 | 차이 |
|-----------|------|------|------|
| BP_YutBoard | 182KB | XXX KB | +/- |
| WBP_YutButton | 188KB | XXX KB | +/- |
| ... | ... | ... | ... |

### 폴더 구조 비교
- 원본에만 있는 파일: ...
- 현재에만 있는 파일: ...

### 기능 구현 상태
- [ ] 말 선택/이동
- [ ] 윷 굴리기
- [ ] 갈림길 처리
- ...
```

### Blueprint 비교 (/compare [blueprint])
```markdown
## Blueprint 비교: [blueprint명]

### 파일 정보
| 항목 | 원본 | 현재 |
|------|------|------|
| 크기 | XXX KB | XXX KB |
| 수정일 | YYYY-MM-DD | YYYY-MM-DD |

### 구조 분석
- 원본 구현 방식: ...
- 현재 구현 방식: ...
- 차이점: ...

### 최적화 제안
- ...
```

## Actions

1. **파일 크기 비교**
   ```bash
   ls -la "E:/UnrealProject/ChaosYut_DM/Content/BluePrint/"
   ls -la "E:/UnrealProject/ChaosYut_New/Content/BluePrint/"
   ```

2. **폴더 구조 비교**
   ```bash
   find "E:/UnrealProject/ChaosYut_DM/Content" -name "*.uasset" | sort
   find "E:/UnrealProject/ChaosYut_New/Content" -name "*.uasset" | sort
   ```

3. **파일 존재 여부 확인**
   - 원본에 있는 파일이 현재에도 있는지
   - 현재에만 있는 새로운 파일은 무엇인지
