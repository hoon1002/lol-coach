# 재개 지점 (리더용)

사용자 요청: 큰 단계 하나가 끝나면 멈추고, 사용자가 "시작"을 보내면 이어서 진행한다.
늘 지킬 규칙(2026-11-05): 모든 기능을 자연스럽게 만든다. 자세한 규칙은 `team/brief.md` 6절에 있고, 팀원 브리핑마다 그 절을 넣는다.

- 현재: **10단계 끝 — C 알갱이로 전체 다시 맞추기(2026-11-05), 사용자 확인 대기(멈춤)**. 기존 시제품 https://claude.ai/artifact/ATE7TitXRq3btFHYFummuh (slug 4c8baa88-4c00-4a86-b4c2-c5d4a74be0c0)에 새 버전 1793870828-84a8 게시(index.html만 바꿈, 그림 11개 그대로). 게시본 사본 team/logs/epic/index-v10-published.html, 이전 판 사본 index-v8-published.html. 흐름: p10-rework(모션 디렉터) → p10-check(비평가 team/ai-tell-c.md: 고치면 통과, 높음 2·중간 9·낮음 4) → p10-fix(15개 모두 고침, 캡처 team/logs/epic/c-fix/) → p10-verify(리더: 바뀐 코드 전부·캡처 확인, 비밀 값 없음, 외부 연결은 글꼴뿐). 실측: CPU 4배 1440 평균 59.9(최저 58.6 첫 화면)·390 60fps, 폰 점 12~12.5px, 움직임 줄이기 폰 무대 1:2로 점 11.5px. 남은 한계: 움직임 줄이기 폰은 글을 보려면 한 번 밀어야 함, 작은 폰 첫 화면 후 그림도 밀어야 함, 포탑 몸통 조각이 조금 길쭉함, C5.07 영혼 칸이 바다 칸과 비슷해 보일 수 있음, 실제 폰 GPU 미측정. 비교 화면 https://claude.ai/artifact/DdkxiN6b5bV8Eao9he26jf. 대시보드 db 버전 70. 남은 결정: D1 수익, D2 대상, 아이템 아이콘 1개, 마스터 기획서 갱신(C 결 반영), 정책(Riot 제품 등록·원화 변형).
- 2단계 산출물: `team/design.md`(433줄), `prototype/index.html`(1,569줄, 화면 6개) → 게시: https://claude.ai/artifact/YHL88yGiZy5ggbyPueE1dT
- 2단계 리뷰 안건 추가: ⑪ 성장 그래프 이중 축 → 위아래 2패널, '원자 지수' 합성 점수 대신 원자별 선(MMR 오인 방지) ⑫ 무료 등급 처방 '잠긴 칸 2개' 노출 여부(planner 결정) ⑬ 법적 고지 영문 원문 글자 대조 ⑭ Hahmlet·IBM Plex Sans KR 라이선스(OFL) 확인 ⑮ 디자이너 폰 스크린샷(`team/logs/proto/m-today.png`) 오른쪽이 잘려 보임 — 로컬 점검에 viewport meta가 없어서 생긴 착시로 추정, 게시본을 폰 너비에서 재확인
- **진행 현황판** (사용자용 실시간 대시보드): https://claude.ai/artifact/C4Xr3o4yuoL5vBCT9qVp1e
  - 원본 데이터: `team/progress/tasks.json`(작업·상태·진행률·가중치), `team/progress/events.json`(소식). 페이지 원본: `team/progress/dashboard.html`
  - 갱신 절차: JSON 파일 수정 → Artifact publish(`url`=위 주소, `file_path`=그 JSON, `asset: true`) → ArtifactData `update` `datasets/<tasks|events>`에 새 `source.url`·`updated` 기록(`if_version` 필수)
  - 디자이너는 마디마다 리더에게 `진행 보고: <작업ID> <진행률>% — …` 메시지를 보낸다 → 받을 때마다 위 절차로 반영
- 사용자가 "시작"을 보내면: 2단계(디자이너) → 3단계(리뷰어) → 4단계(마스터 기획서) 순서로 진행한다. 각 단계가 끝날 때마다 다시 멈춘다.
- 1단계 팀원 세션(planner·researcher·curriculum·engine)은 idle 상태로 남아 있다. 3단계 리뷰 후 수정 지시는 새 세션이 아니라 이 세션들에 메시지로 보낸다.
- 전체 맥락: `team/brief.md`

## 1단계 검증 결과 (리더)

| 산출물 | 분량 | 브리프 대비 | 판정 |
|---|---|---|---|
| `team/plan.md` | 777줄 | 11개 항목 + 부록 A~C 모두 있음 | 통과 |
| `team/research.md` | 593줄 | A~F + G(미확인 모음), 사실마다 URL·확인일 | 통과 |
| `team/curriculum.md` | 879줄, 원자 233개 | a~f + g(엔진 ID 대응표), 용어집 57개 | 통과 |
| `team/lessons/01~03` | 343·359·327줄 | 템플릿·본문·드릴·퀴즈 3·지표 모두 있음. 02의 백무빙 사례 수치가 engine.md 7.4와 일치 | 통과 |
| `team/engine.md` | 936줄 | 10개 절 + 백무빙 워크스루(원시 데이터 → 앱 카드) | 통과 |

보안 점검: 비밀 값 패턴 0건, `.env` 없음, 환경변수는 이름(`RIOT_API_KEY`, `ANTHROPIC_API_KEY`)으로만 언급. 결제·발송·배포·계정 생성 없음.
기획자가 말한 공유 노트 `planner-handover.md`는 작업 폴더에서 찾지 못함. 같은 결정 내용은 `plan.md`에 들어 있으므로 문제없음.

## 3단계 리뷰 안건 (리더가 1단계 검증 중 찾은 것)

1. **원자 ID 통일**: engine.md는 임시 ID(`LANE.TRADE.CD_WINDOW` 등), plan.md는 예시 ID를 씀 → `curriculum.md`의 `L1.*` 체계를 정본으로 하고 g절 대응표대로 치환. 지표 ID 표기(`METRIC.{원자ID}:{지표명}`)도 확정.
2. **아타칸**: brief는 정정함. engine.md 5.2의 "[검증 필요]"는 research.md D장 기준으로 확정 반영.
3. **`WARD_PLACED` 위치 필드**: plan.md는 있다고 가정, engine.md는 없다고 봄 → 사실 확인 후 시야 원자(`B0.MAP.FOG.02` 등)의 해상도 확정.
4. **경쟁 포지셔닝 갱신**: YOUR.GG가 2026-10-14 영상 AI 코칭(타임스탬프 복기)을 출시함(research.md 0장). plan.md 1.3 포지셔닝·차별점을 "닫힌 루프 + 근거 공개 + 한국어 교과서"로 무게중심을 옮겨야 함.
5. **정책 반영**: plan.md 8.1은 정책 원문을 못 열었다고 적음 → research.md B장(확인됨)으로 갱신. MMR 계산기 금지는 확정, "결정 지시 금지", 메모리 접근 차단, 리플레이 다운로드 시간당 10회를 반영.
6. **Claude 가격**: engine.md 7.3·10.2의 모델별 단가는 리더가 검증하지 못함 → 공식 가격 페이지로 재확인하거나 "가정"으로 표시.
7. **26.10 미니언 어그로 변경**(lesson 02, curriculum `L1.WAVE.PULL.01`): research.md에 없는 사실 → 출처 재확인.
8. **리텐션 설계 충돌**: plan.md는 일일 스트릭 금지(주 단위 리듬), research.md는 듀오링고식 최소 단위 스트릭 근거를 제시 → 리뷰어가 정리하고, 필요하면 사용자 결정 사항으로 올림.
9. **무료 등급 범위**: plan.md "진단 하루 3게임" vs research.md 체스닷컴식 "복기 하루 1판" 제안 → 하나로 정리.
10. **정석 카드 승률 Δ 필드**: curriculum f.2가 engine 6.6 스키마에 승률 필드 추가를 요청함.

## 3단계 결과 (리더 재검증)

- review.md: 지적 35건(치명 1·높음 10·중간 17·낮음 7) → 5명 전원 반영, 각 문서 끝에 `## 변경 이력 — 3단계 리뷰 반영` 표. 후속: 시야 문장 60초, 정석 문구 "어그로 안전 위치에서 한 걸음", 손익분기는 무료/유료 분리 원가(무료 MAU당 $0.02~0.03, 유료 약 $1.1)로 재계산.
- 대조 결과: 임시 원자 ID 잔존 0, "12:43" 예시는 각주만, 비밀 값 0, 프로토타입 외부 리소스는 Google Fonts뿐. 프로토타입 리뷰 반영판 재게시(Version 2).
- 사용자 결정 대기: D1 무료 범위 · D2 리텐션 리듬 · D3 사람 비용 집행 · D4 '프로 분석' 약속 범위 · D5 초 단위 분석 투자 게이트 (review.md 4장, 현재 리뷰어 추천안을 `[잠정]` 반영)
- 4단계에서 정리할 작은 것: 프로토타입 성장·재측정 예시가 여전히 `L1.CS.TURRET.01`(이제 R4 전용, MVP 제외)을 쓰고 있음, design.md 206줄 "plan과 다르게 그린 점"은 plan이 채택했으므로 문구 갱신
