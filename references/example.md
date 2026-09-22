# 예: 헬스체크 엔드포인트 추가

사용자: `/planning-goals 헬스체크 엔드포인트 추가`

## 조사

라우터 파일 위치, 테스트 러너, 서버 포트를 코드에서 읽는다. 셋 다 근거가 있으므로 묻지 않는다.

## 질문 (1개)

> 응답에 앱 버전도 넣을까요? (추천: 아니오. status 만 돌려주고 버전은 범위 밖으로 둡니다)

사용자: 추천대로.

끝난 상태(200 과 `{"status":"ok"}`), 증명(`npm test`, `curl`), 하지 않을 것(버전)이 채워졌으므로 질문을 멈춘다.

## `.goal/healthcheck.md`

```markdown
# Goal: 헬스체크 엔드포인트 추가
Date: 2026-09-17

## 완료 조건
.goal/healthcheck.md 의 모든 단계와 DoD 가 [x] 완료로 표시되고 각 항목의 검증 결과가 출력에 있음. git 은 실행하지 않고 명령만 제시.

## 만들 것
`GET /health` 가 `{"status":"ok"}` 를 200 으로 돌려준다. 기존 라우터 파일에 라우트 하나와 테스트 하나를 더한다. 다른 라우트와 응답 형식은 바꾸지 않는다.

## 조사 결과
| 항목 | 값 | 근거 |
|---|---|---|
| 라우터 파일 | `src/routes.js` | 확인함 — `grep -rn "app.get" src/` |
| 테스트 러너 | jest, `npm test` | 확인함 — `package.json` scripts.test |
| 서버 포트 | 3000 | 확인함 — `src/server.js:12` |
| 작업 트리 | 깨끗함 | 확인함 — `git status --short` 빈 출력 |

## 범위 밖
- 응답에 앱 버전 포함 — 다음 목표로

## 제약
- 기존 라우트 파일 외 수정 금지 — `git status --short` 가 조사 시점 스냅샷(빈 출력) 대비 `src/routes.js`, `test/health.test.js` 만 추가

## 단계
### 1. 엔드포인트 구현
- 동작: `src/routes.js` 에 라우트 추가, `test/health.test.js` 작성
- 검증: `npm test` 전부 통과 — 출력 붙여 넣기. `git status --short` 출력 붙여 넣기
- [ ] 완료

### 2. 커밋 명령 제시
- 동작: `git add`/`git commit` 을 코드 블록으로
- 검증: 코드 블록이 출력에 있음
- [ ] 완료

### 3. DoD 검증
- 동작: 서버를 띄우고 DoD 의 검증 명령 실행
- 검증: 아래 DoD 항목의 출력 붙여 넣기
- [ ] 완료

## DoD
- [ ] `GET /health` 가 응답함 — 검증: `curl -s -o /dev/null -w '%{http_code}' localhost:3000/health` 출력이 `200`
```

## `.goal/healthcheck-test.md`

```markdown
# TEST: 헬스체크 엔드포인트 추가
Date: 2026-09-17
대상: .goal/healthcheck.md

## 완료 조건
.goal/healthcheck-test.md 의 모든 케이스가 [x]·실패·판정 불가 중 하나로 표시되고 각 케이스의 실행 출력이 있으며 판정 절이 채워짐. 코드는 수정하지 않음.

## 사전 조건
- 서버 실행 중 — 확인: `curl -s -o /dev/null -w '%{http_code}' localhost:3000/` 가 응답

## 제약
- 코드 수정 금지 — 실패는 기록만 한다. 고치는 일은 구현 goal 을 다시 열어서 한다

## 케이스
| # | 분기 | 입력 | 실행 | 기대 | 판정 |
|---|---|---|---|---|---|
| 1 | 정상 | GET /health | `curl -s -i localhost:3000/health` | 첫 줄 `200`, 본문 `{"status":"ok"}` | [ ] |
| 2 | 메서드 불일치 | POST /health | `curl -s -o /dev/null -w '%{http_code}' -X POST localhost:3000/health` | `404` 또는 `405` | [ ] |
| 3 | 기존 라우트 영향 없음 | GET / | `curl -s -o /dev/null -w '%{http_code}' localhost:3000/` | 구현 전과 같은 코드 | [ ] |

## 결과
- (루프가 채움)

## 판정
- [ ] 전체 통과 — 검증: 결과 절에 실패 0건, 판정 불가 0건
```

## 마지막 메시지

```
.goal/healthcheck.md 와 .goal/healthcheck-test.md 를 만들었습니다. 구현이 끝난 뒤 TEST 를 입력하세요.
/goal .goal/healthcheck.md 의 모든 단계와 DoD 가 [x] 완료로 표시되고 각 항목의 검증 결과가 출력에 있음. git 은 실행하지 않고 명령만 제시.
/goal .goal/healthcheck-test.md 의 모든 케이스가 [x]·실패·판정 불가 중 하나로 표시되고 각 케이스의 실행 출력이 있으며 판정 절이 채워짐. 코드는 수정하지 않음.
```

작은 목표라 참조·결정·변경 기록 절은 쓰지 않았다. 근거 있는 사실이 3개라 조사 결과 절은 썼다.
