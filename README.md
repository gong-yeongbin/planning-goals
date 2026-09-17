# planning-goals

Claude Code 의 `/goal <조건>` 에 넣을 조건문과 실행 계획 파일을 만드는 스킬.

`/goal` 은 조건이 충족될 때까지 턴을 반복한다. 평가 모델은 명령을 실행하지 못하고 Claude 의 출력만 읽는다. 그래서 조건은 출력에서 확인 가능해야 하고, 단계마다 증거가 남아야 한다. 이 스킬은 목표 한 줄을 받아 그 형태로 정리한다.

## 설치

```bash
# 프로젝트 로컬
npx skills@latest add gong-yeongbin/planning-goals

# 전역
npx skills@latest add gong-yeongbin/planning-goals -g
```

수동 설치: `SKILL.md` 를 `~/.claude/skills/planning-goals/SKILL.md` (전역) 또는 `<project>/.claude/skills/planning-goals/SKILL.md` (프로젝트) 에 둔다.

## 사용

```
/planning-goals <목표 한 줄>
```

또는 "goal 정리해 줘", "goal 조건 만들어 줘" 같은 말로 발동한다.

흐름: 코드·문서로 알 수 있는 건 조사 → 모르는 것만 한 번에 하나씩 질문(추천 답 동봉) → `<cwd>/.goal/<슬러그>.md` 작성 → 붙여 넣을 `/goal ...` 한 줄 제시.

`/goal` 은 슬래시 명령이라 사용자가 직접 입력한다.

## 파일

| 파일 | 내용 |
|---|---|
| `SKILL.md` | 스킬 본문 |
| `baseline.md` | 스킬 없이 실행한 기준선 관찰 (superpowers:writing-skills 의 RED 단계 기록) |
