# GRIMOIRE 조사 보고서 — 경쟁·정책·데이터·게임 상태·시장

> 작성: 조사원(researcher) · 기준일 2026-11-02 · 확인일 2026-11-02~03 (항목별 표기) · **개정 2026-11-03: 3단계 교차 리뷰 반영**(맨 끝 '변경 이력' 표)
> 독자: 기획자(plan.md) · 교과서 설계자(curriculum.md) · AI 아키텍트(engine.md) · 리뷰어(review.md)

**표기 규칙**
- `[공식]` Riot·서비스 운영사 공식 페이지에서 직접 확인 · `[2차]` 언론·블로그·커뮤니티 문서 · `[자사 주장]` 해당 서비스가 스스로 내세운 수치(검증 안 됨)
- `[검증 필요]` 출처가 약하거나 서로 엇갈림 · `[확인 못 함]` 찾지 못함 · `[계산]` 출처 수치로 조사원이 계산한 값
- 패치 태그: 게임 패치는 `26.xx`, Data Dragon 버전은 `16.xx.1`로 표기(둘은 같은 패치. 예: 게임 26.21 = DDragon 16.21.1)
- 인용은 짧은 구절만 쓰고 나머지는 한국어로 요약했다. 정책 원문은 링크에서 직접 확인할 것.

---

## 0. 핵심 요약 (먼저 읽을 것)

1. **가장 가까운 경쟁자는 YOUR.GG(젠지)다.** 2026-10-14 '영상 AI 코칭'을 출시했다: 장면을 고르면 AI가 영상을 보고 피드백을 주고, 답변 속 시간을 누르면 영상의 그 순간으로 이동하며, 후속 질문도 받는다. 월 7,900~24,900원. 우리가 노리는 "타임스탬프 복기" 자체는 더 이상 빈자리가 아니다. 남은 빈자리는 **교과서 원자 개념 ↔ 측정 지표 ↔ 처방 드릴의 닫힌 루프**와 **근거(표본·신뢰도) 공개**다. (A장)
2. **Riot은 2026년 9~10월 서드파티 접근을 크게 조였다.** 알 수 없는 앱의 게임 메모리 접근을 2026-10-06부터 기본 차단했고(승인 앱 유예는 2027-04까지), 이유로 "AI 보조 리버스 엔지니어링"을 들었다. 26.21부터 리플레이 다운로드는 **시간당 10회**로 제한된다. 메모리 읽기·대량 리플레이 수집에 기대는 설계는 피해야 한다. (B·C장)
3. **실시간 분석은 정책상 금지 쪽이다.** Riot 고객지원 문서는 실시간으로 플레이어 대신 게임 플레이를 분석하는 것을 금지 예시로 들고, 먼저 플레이하고 분석·복기는 나중에 하라는 입장을 밝힌다. 개발자 정책은 적 궁극기 쿨다운 추적을 "자동이든 수동이든" 금지 예시로 명시한다. GRIMOIRE의 핵심 가치(사후 복기·처방)는 정책 방향과 맞는다. (B장)
4. **match-v5 타임라인은 1분(60,000ms) 간격 프레임이다.** 위치·골드·XP는 1분마다, 이벤트(킬·구매·와드·오브젝트)는 ms 타임스탬프로 남는다(단 와드 이벤트에는 위치가 없다). "백무빙 1회" 수준은 API만으로 **측정할 수 없다.** 그 해상도에는 리플레이 렌더링 + 컴퓨터 비전이 필요하고, 리플레이는 패치가 바뀌면 재생할 수 없으며 클라이언트에 최근 20판만 남는다. (C장)
5. **프로 경기 공식 데이터는 GRID 독점이고 상업 계약 전용이다.** LCK 데이터는 "미디어 용도 한정"으로 표기돼 있다. 비상업용 데이터 포털은 아직 열리지 않았다. 초기 '정석 엔진'은 KR 상위 솔로랭크(공식 API) 위주로 설계해야 한다. (C장)
6. **2026 시즌**: 26.1(2026-01-07~08)에서 **역할 퀘스트가 5개 포지션 전체로 확대**됐고, **아타칸과 '힘의 위업(Feats of Strength)'이 제거**됐으며, 치명타 기본 피해가 200%로 돌아왔다. 현재 최신 패치는 **26.21**(2026-10-20 노트)이고, 26.22는 2026-11-04 예정이라는 2차 출처가 있다. (D장)

---

## A. 경쟁 서비스 분석

### A-1. 한눈에 보는 비교표 (확인일 2026-11-02)

| 서비스 (운영사) | 유형 | 핵심 기능 | 데이터 소스 | 가격 | 타임스탬프 미시 피드백 |
|---|---|---|---|---|---|
| **YOUR.GG** (젠지이스포츠글로벌아카데미, KR) | AI 코칭 + 오버레이 | PLAYREPORT(AI 진단), **영상 AI 코칭**(2026-10-14), 게임 전 AI 보이스 코칭, 인게임 의사결정 보조 오버레이, 적 정글 감지 알림, 디스코드 주간 리포트 | Riot API + 데스크톱 앱(클라이언트 연동) + 사용자 녹화 영상 | PLUS 7,900원 / PRO 9,900원 / PRO+ 24,900원 (월) `[공식]` | **있음(장면 단위)**: 답변의 시간을 누르면 영상 해당 순간으로 이동. 수치화된 손익(예: 체력 교환 −180) 제공 여부는 `[확인 못 함]` |
| **OP.GG** (OP.GG, KR) | 전적·통계 + 데스크톱 + 코칭 마켓 | 전적 검색, 빌드·티어, 데스크톱 앱·오버레이, Spectate, **Gigs**(사람 코칭 마켓, 메뉴명 "Lessons") | Riot API | 프리미엄 페이지 404, 가격 `[확인 못 함]` | AI 미시 피드백 `[확인 못 함]`(사람 코치 마켓으로 대체) |
| **DeepLoL** (GameEye Corp., KR) | 전적·통계 | 전적 검색, **AI Score**, 티어·빌드, 랭킹, 프로/스트리머 등록 | Riot API | `[확인 못 함]` | `[확인 못 함]` |
| **Mobalytics** (Gamers Net, US) | 지표·코칭형 통계 | **GPI**(플레이어 역량 지수), Live Companion(매치업 조언), 챔피언 콤보, 인게임 챌린지, **Smart Highlights**(상위 5개 플레이 자동 추출) | Riot API + 데스크톱 앱 | Plus $9.99/월, $24.99/3개월, $69.99/년 `[공식]` | 부분적: 하이라이트 5개를 자동 추출. 처방형 해설 여부 `[확인 못 함]` |
| **U.GG** (US) | 통계·빌드 | 빌드·카운터·티어, 개인 카운터·개인 티어리스트, 디스코드 경기 요약 | Riot API | PLUS 월 $3.99, 연 $29.88($2.49/월), 7일 무료 `[공식]` | 없음 |
| **LoLalytics** | 통계 | 챔피언 승률·픽·밴, 패치 대비 변화, 매치업·빌드 | 랭크 게임 전수 분석(자사 주장) | 유료 플랜 표시 없음 `[공식]` | 없음 |
| **Blitz** | 데스크톱·오버레이 | 빌드·룬 자동 적용, 오버레이 | Riot API + 클라이언트 | Premium 월 $3.99 / 연간 월 $2.67 상당 `[공식]` | 없음 |
| **Porofessor** (Overwolf) | 라이브 상대 분석 | 로딩 화면 상대 전적·성향 | Riot API | 무료 + 월 구독 프리미엄(가격 `[확인 못 함]`) `[2차]` | 없음 |
| **Skill Capped** | 영상 강의 | 코스형 강의(챔피언·역할), "Smurf Commentaries" | 자체 제작 영상 | "월 $8.25부터", 14일 환불 보장 `[공식]` | 없음(일반 강의) |
| **ProGuides** | 코칭 마켓 | — | — | **2024-05-29 코칭 플랫폼 종료** `[2차]` | — |
| **GGQ** (지지큐컴퍼니, KR) | AI 코칭 | 밴픽 추천, 게임 후 하이라이트 영상 + 전투별 대응 코칭, 5개 부문 기록 | Riot API + **AI 객체 인식** | `[확인 못 함]` (2023-11 PC방 2,500여 곳 오픈베타) | 전투 단위 코칭 `[2차]` |
| **STATUP.GG** (Gamer Republic, KR 기업 디렉터리 등재) | **실시간** AI 음성 코치 | 게임 중 음성 피드백 + 게임 후 리포트 | 화면 인식(vision AI) | Beginner 무료, 상위 티어 1개월 체험 `[공식]` | 실시간(정책 리스크 — B장) |
| **iTero** | 드래프트·오버레이·매크로 | 드래프트 추천, 정글 리스폰 타이머·아군 궁 쿨 오버레이, Macro Coach(사후) | `[확인 못 함]`(FAQ 미기재) | 무료 + 유료(광고 제거·AI 모델 설정) | 매크로 실수 목록. 타임스탬프 `[확인 못 함]` |
| **UpForge** | AI 사후 분석 | 데스 패턴·오브젝트 교환·시야 누수 리포트, 판당 고칠 습관 1개 | **match-v5 + timeline** | 첫 분석 무료, Plus/Pro 가격 미공개 | 타임스탬프 표기 `[확인 못 함]` |
| **VOD Coach** (오픈소스, GitHub) | AI VOD 리뷰 | 실수·좋은 플레이마다 클릭 가능한 타임스탬프 + 대안 제시, 미니맵·HUD 인식(v1.4) | 사용자 녹화 영상 + 사용자 본인 Gemini 키 | 무료(구글 무료 할당량 한도) | **있음** |
| **Insights.gg** | VOD 협업 도구(비AI) | 영상 업로드, 타임스탬프·드로잉·코멘트, 공동 리뷰 | 사용자 영상 | 무료 녹화 | 사람이 직접 표시 |
| **사람 코칭** (Metafy 등) | 1:1 VOD 리뷰 | 코치가 리플레이를 직접 분석 | — | 판/세션당 약 $12~$60 `[2차]` | 있음(사람) |

출처: YOUR.GG https://your.gg/ko/kr/membership , https://gengesports.notion.site/2026-10-14-AI-3f96548b309e80d58d37e7b43e7580c6 · OP.GG https://op.gg/desktop/en , https://op.gg/lol/premium · DeepLoL https://www.deeplol.gg/ · Mobalytics https://mobalytics.gg/lol/glp/plus , https://mobalytics.gg/ · U.GG https://u.gg/plus · LoLalytics https://lolalytics.com/ · Blitz https://blitz.gg/premium · Porofessor https://go.overwolf.com/porofessor · Skill Capped https://www.skill-capped.com/lol/landing · ProGuides https://www.dodge.gg/valorant/news/is-proguides-worth-it-for-valorant-2026 , https://app.dealroom.co/companies/proguides · GGQ http://www.ilovepcbang.com/news/articleView.html?idxno=101421 · STATUP.GG https://statupgg.itch.io/statupgg , https://welcon.kocca.kr/en/directory/company/gamer-republic-inc--4181 · iTero https://www.itero.gg/ · UpForge https://upforge.gg/lol/coaching · VOD Coach https://github.com/TsakasOptimizations/VOD_Coach · Insights.gg https://insights.gg/insights-web · Metafy 세션 목록 https://metafy.gg/@apples/sessions/replay-review-tEnneV12Mob (모두 확인일 2026-11-02)

### A-2. 서비스별 상세 메모

**YOUR.GG (젠지) — 직접 경쟁자 1순위** (확인일 2026-11-02)
- 운영 주체: 젠지이스포츠글로벌아카데미 유한회사(사이트 하단 사업자 정보). 홈 배너: "업데이트 10/16 — 내 플레이에 답하는 AI 코칭". https://your.gg/ko/kr/home
- **영상 AI 코칭 (2026-10-14 패치노트)**: 장면을 고르고 '내 플레이를 코칭해 줘'를 누르면 AI가 영상을 보고 그 상황에서 어떤 선택을 했는지 짚는다. 답변에 표시된 시간을 누르면 영상의 그 순간으로 이동한다. 사용자가 당시 의도(예: 아군 정글러 합류를 기대하고 싸웠다)를 설명하면, 교전 시작 시점과 아군이 실제로 합류할 수 있었던 시점의 차이를 더 자세히 짚는 대화형 복기를 한다. https://gengesports.notion.site/2026-10-14-AI-3f96548b309e80d58d37e7b43e7580c6
- **멤버십(월간 가격)**: PLUS 7,900원(영상 AI 코칭 기본 사용량, 2GB 저장공간, 1개 챔피언 리포트 무제한, 게임 전 AI 보이스 코칭 월 20회) / PRO 9,900원(2배 사용량, 4GB, 라인·챔피언 리포트 무제한, 보이스 월 50회) / PRO+ 24,900원(5.5배, 10GB, 보이스 무제한). 무료 등급: 협곡 인게임 오버레이, 게임 전 가이드, 게임 후 전적 분석. https://your.gg/ko/kr/membership
- **인게임 오버레이 설명(요약)**: 이벤트 타임라인으로 교전 우위, 핵심 아이템 구매 시점, 맞라이너의 라인 복귀 시점, 적이 핵심 아이템을 갖췄는지를 보여 준다고 소개한다. 같은 페이지.
- **적 정글 감지 알림 (2026-06-19)**: 라인전 중 적 정글러의 레벨·CS·아이템 변화가 감지되면 알림음과 음성으로 알린다. 14분까지만 제공한다. 운영사는 정확한 위치를 알려 주는 기능이 아니라 미니맵 확인 습관을 들이는 코칭 알림이라고 설명한다. https://gengesports.notion.site/2026-06-19-3846548b309e80328b9cc70cfb14b70c → 정책 회색지대 사례로 B-4에서 다룬다.
- 자사 주장: 이용자 1만 명, 리포트 27만 개(멤버십 페이지). 스트리머 후기 다수.
- 2025-11-11 패치에서 디스코드 AI 챗봇 'YOUR BOT'을 추가했고, 답변마다 출처와 데이터 링크를 붙인다고 내세운다 `[공식]` https://gengesports.notion.site/2025-11-11-Update-YOUR-BOT-2a86548b309e809e8059d02bca5c8800
- PLAYREPORT는 해당 챔피언·라인 조합으로 최근 랭크 최소 10판이 필요하다는 2차 설명이 있다 `[2차]` https://creati.ai/ko/ai-tools/your-gg/
- **우리 관점의 약점(추정, 검증 필요)**: (1) 커리큘럼(교과서)이 없어 피드백이 "이번 장면" 단위로 끝나고 체계적 학습 경로로 이어지지 않는다. (2) 피드백의 통계 근거(표본·승률)를 공개하는지 확인되지 않는다. (3) 영상 AI는 사용자가 녹화한 영상에 의존하므로 프레임 단위 수치(쿨다운·체력 교환)를 정량화하는지는 불명.

**OP.GG** (확인일 2026-11-02)
- 상단 메뉴: Desktop, Duo, TalkG, Esports, Gigs, Streamer Overlay, "Lessons"(→ gigs.op.gg 상품 목록). 배너: "Rank Up in 3 Days! Challenger Coaching" → 사람 코치 마켓으로 학습 수요를 흡수한다. https://op.gg/lol/premium
- 데스크톱 앱: Windows/macOS, "Riot Games Policy compliant" 표기, 2026-10-28 v3.2.0. https://op.gg/desktop/en
- 2025-08 스펠·스킬 트래커 오버레이를 Riot 정책 변경을 이유로 제거했다는 보도가 있다 `[2차, 경쟁사 작성 블로그]` https://hexgate.app/blog/riot-third-party-apps/
- 한국 PC방 지표에서는 OP.GG 관련 수치를 찾지 못했다 `[확인 못 함]`.

**DeepLoL** (확인일 2026-11-02)
- 페이지 제목 "전적 검색 딥롤 라이엇 아이디 태그 AI Score 검색", 운영 GameEye Corp., "프로/스트리머 등록" 메뉴가 있다. 기능 세부·가격은 `[확인 못 함]`. https://www.deeplol.gg/

**Mobalytics** (확인일 2026-11-02)
- 홈 문구: 10,000,000+ 플레이어, "Mobalytics를 쓰는 플레이어가 27% 더 많이 승급" `[자사 주장]`, "LCS 아마추어 씬 공식 데이터 파트너". 개선 루프를 Before/During/After/Between 게임 4단계로 구성한다. https://mobalytics.gg/
- Plus 기능: Live Companion(챔피언 대 챔피언 매치업), 챔피언 콤보, GPI 진행 그래프, 인게임 챌린지, Smart Highlights(상위 5개 플레이 자동 식별). 가격 $9.99/월 · $24.99/3개월 · $69.99/12개월. https://mobalytics.gg/lol/glp/plus
- 시사점: **"개선 루프"를 마케팅 언어로 이미 쓰고 있다.** 그러나 루프의 '처방' 칸이 강의 커리큘럼으로 이어지는지는 `[확인 못 함]`.

**Skill Capped** (확인일 2026-11-02)
- '엘로 지옥 탈출 보장'을 내건 랜딩 문구, 강사는 상위 0.02%라는 주장, 월 $8.25부터, 14일 환불 보장, 구독자와 비구독자의 티어 상승 비교 그래프 `[자사 주장]`. 개인 경기 분석 기능은 없다. https://www.skill-capped.com/lol/landing
- 시사점: **강의형은 "보장(guarantee)" 마케팅과 연간 구독으로 매출을 만든다.** 개인화가 없다는 게 약점이다.

**신생 AI 코칭 공통 패턴** (2024~2026)
- 데이터 경로는 둘로 나뉜다: (1) **match-v5 타임라인 기반**(UpForge 등): 싸고 정책상 안전하지만 1분 해상도에 머문다. (2) **영상·화면 인식 기반**(YOUR.GG 영상 코칭, GGQ, STATUP.GG, VOD Coach): 미시 해상도는 얻지만 녹화·연산 비용이 크고, 실시간이면 정책 리스크가 있다.
- LLM을 붙인 대화형 복기(YOUR.GG, VOD Coach)가 2026년 기준 표준이 되어 가고 있다.

### A-3. 우리가 차지할 빈자리 (White Space)

| # | 빈자리 | 근거 (경쟁 현황) | GRIMOIRE 대응 |
|---|---|---|---|
| W1 | **교과서 ↔ 진단 ↔ 처방 ↔ 재측정의 닫힌 루프** | 통계형(OP.GG·U.GG·LoLalytics)은 '무엇을 고를까', 강의형(Skill Capped)은 일반 원리, AI 코칭형(YOUR.GG·UpForge)은 '이번 판 피드백'에서 끝난다. 원자 개념 단위로 학습 경로를 잇는 곳은 확인 범위에서 없음 | 원자 개념 ID = 엔진 지표 ID (brief의 1:1 규칙) |
| W2 | **근거 투명성**(표본 수·승률 차이·신뢰구간·예외 조건) | 챔피언 통계 표본은 LoLalytics 등이 보여 주지만 "이 판단을 하면 승률이 얼마나 바뀌나" 같은 결정 단위 근거와 신뢰도를 공개하는 서비스는 확인 범위에서 없음 | 정석 카드마다 n, Δ승률, 신뢰도, 예외 표시 |
| W3 | **아이언~골드 학습자를 위한 한국어 체계 커리큘럼** | 한국 서비스는 통계·AI 진단 중심이고 커리큘럼은 없음. 해외 강의형은 영어 | 한국어 커뮤니티 용어 기반 마도서 |
| W4 | **정량화된 미시 손익**("백무빙 1회 → 체력 교환 −180") | YOUR.GG 영상 코칭은 장면 해설은 하지만 수치화 여부가 불명. 타임라인 기반 서비스는 1분 해상도라 불가 | 리플레이 렌더 + CV로 체력·쿨다운·거리 추정(C-3, → engine.md) |
| W5 | **정책 안전 설계를 강점으로** | 2026-10 메모리 차단, 2027-04 유예 종료, 실시간 분석 금지. 오버레이형 경쟁사는 규제 리스크를 안고 있음 | 인게임 개입 0, 사후 분석 전용 |
| W6 | **드릴(수련)과 리텐션 장치** | 통계·코칭 서비스에 스트릭·간격 반복 같은 학습 리텐션 장치는 Mobalytics의 챌린지 정도만 확인됨 | 주간 학습 리듬 + 낮은 최소 단위(드릴 1개 또는 레슨 1개). 일일 스트릭은 옵트인 실험으로만(E-4) |

> 주의: W4는 YOUR.GG가 가장 빨리 따라올 수 있는 영역이다. 차별화의 무게중심은 W1·W2(교과서와 근거)에 둬야 한다.

---

## B. Riot 개발자 정책

### B-1. API 키 종류와 레이트 리밋

| 키 | 발급 | 용도 제한 | 레이트 리밋 | 비고 |
|---|---|---|---|---|
| Development | 로그인 시 자동 발급 | 실험·프로토타입 전용, 공개 서비스 불가 | 포털 문서에 수치 없음. API 약관(2013 문구)은 "10초에 10회"라고 적고, 서드파티 가이드는 "1초 20회·2분 100회"라고 적음 → **출처 엇갈림 `[검증 필요]`** | **24시간마다 비활성화**, 매일 재발급해야 함 |
| Personal | 제품 등록(검증 불필요) | 본인·소규모용. **공개 서비스 불가(공개 알파·베타 포함)**, Tournaments API 불가, 리밋 상향 불가 | **1초 20회, 2분 100회** | — |
| Production | 제품 등록 + 검증 + 심사 | 대규모·공개 서비스용, Tournaments API 가능 | **시작값: 10초 500회, 10분 30,000회** | 대체로 **작동하는 프로토타입**이 필요 |

- 레이트 리밋은 3종: **Application**(키·리전 단위, 모든 호출 합산, 정적 데이터 제외), **Method**(엔드포인트·키·리전 단위), **Service**(서비스·리전 단위, 모든 앱 공유). 일부 하위 서비스는 X-Rate-Limit-Type 헤더 없이 429를 줄 수 있다.
- 메서드별 수치(예: match-v5)는 공식 문서에서 찾지 못했다. 응답 헤더 `X-Method-Rate-Limit`로 확인해야 한다는 서드파티 설명이 있다 `[2차]`.
- 출처: https://developer.riotgames.com/docs/portal , https://developer.riotgames.com/terms , https://www.mintlify.com/ValentinSetti/tracker-lol-backend/api/rate-limits , https://github.com/kodbilenadam/rito/wiki (확인일 2026-11-02)

### B-2. 제품 등록·승인 절차

1. 정책을 읽는다 → 2. 포털 메인에서 Register Product → 3. "대규모 제품" 또는 "개인 프로젝트" 선택 → 4. 온라인 양식 작성 → 5. 제품 검증 → 6. Developer Relations 심사 후 포털 메시지로 승인·거절 통보.
- 심사 기준: **법 준수, 플레이어에게 측정 가능한 이익, 품질**. 처리 기간은 프로젝트·대상 리전마다 다르다. **프로젝트마다 별도 등록·승인**이 필요하다.
- 모든 제품은 등록·감사(audit)를 받아야 하고, **새 기능·변경도 제품 페이지에서 다시 감사**받아야 한다. LCU(League Client API)를 쓰면 그 사용도 등록해야 한다.
- **Production 키 하나에 제품 하나.** 키를 조직 밖과 공유하지 말고, 배포 바이너리에 키를 넣지 말 것. → 데스크톱 컴패니언은 Riot API를 직접 부르지 말고 우리 서버를 거쳐야 한다.
- 출처: https://developer.riotgames.com/docs/portal , https://developer.riotgames.com/policies/general (정책 최종 갱신 2025-05-29 표기, 확인일 2026-11-02)

### B-3. 수익화 규칙

| 규칙 | 내용 | 출처 |
|---|---|---|
| 수익화 가능 조건 | 제품 상태가 **Approved 또는 Acknowledged**일 때만 | 일반 정책 |
| **무료 등급 의무** | 플레이어용 무료 등급이 반드시 있어야 함(광고 포함 가능) | 일반 정책 |
| 유료 콘텐츠 조건 | **변형적(transformative)**이어야 함: 새로운 정보·미감·통찰·이해를 더할 것 | 일반 정책 |
| 허용되는 과금 | 구독·후원·크라우드펀딩 / 토너먼트 참가비 / 법정화폐로 되돌릴 수 없는 재화 | 일반 정책 |
| 금지 | 플레이어 착취·불공정 과금(판단은 Riot), **베팅·도박**, **Riot 자산(인게임·로딩 화면·Riot 클라이언트) 내 광고** | 일반 정책 (2025-05-29 개정에서 인게임 오버레이 광고 금지) |
| 블록체인 | 암호화폐·블록체인 금지 | 일반 정책 |
| 게임 정보 판매 | API 약관상 Game Information 접근에 과금하려면 **Riot의 사전 서면 승인** 필요(약관 문구가 오래됨, `[검증 필요]`: 현재 일반 정책과의 관계) | API 약관 |

- 출처: https://developer.riotgames.com/policies/general , https://developer.riotgames.com/terms , https://eloking.com/blog/lol-riot-bans-overlay-advertisements `[2차]` (확인일 2026-11-02)
- 시사점: **구독 모델 + 무료 등급**은 허용된다. 유료 등급은 "새로운 통찰"(진단·처방·정석 근거)이어야 하고, 단순 데이터 재판매는 위험하다.

### B-4. 게임 무결성 — '부당한 이점' 규정 (가장 중요)

**개발자 정책(LoL 문서·일반 정책)의 요지** (https://developer.riotgames.com/docs/lol , https://developer.riotgames.com/policies/general , 확인일 2026-11-02)
- 게임 클라이언트에 없는 정보로 경쟁 우위를 주면 안 된다. 예시로 **적 궁극기 쿨다운 추적을 "자동이든 수동이든"** 명시한다.
- 게임의 목표(넥서스 파괴)를 바꾸면 안 되고, 치트처럼 일부 플레이어에게만 우위를 주면 안 된다.
- 게임 결정(빌드·조합·챔피언)의 **다양성을 줄이면 안 되고, 결정을 없애면 안 된다.** 중요한 결정을 강조하고 **여러 선택지를 제시하는 것은 허용**된다.
- 공식 랭크를 대체하는 시스템 금지: **MMR·ELO 계산기 금지.**
- 게임이 의도적으로 숨긴 플레이어를 식별·분석하면 안 된다. 보이는 정보로 합리적으로 식별할 수 없는 플레이어의 익명성을 해제하면 안 된다.
- **비승인 사례(LoL)**: 증강·아레나 아이템 승률 표시(웹·앱·오버레이 전부) / **그 게임 세션에서 플레이어가 몰랐던 정보 제공** / **플레이어 결정을 지시(dictate)하는 앱** / 사용자 설정 게임 전적 공개(옵트인 또는 RSO 본인 열람만 허용).
- **승인 사례(LoL)**: 게임 전에 알 수 있는 정적 데이터 오버레이 / **본인 경기 기록과 집계 통계를 보는 훈련 도구** / 특정 플레이어가 아닌 집계 통계 / 공식 래더 리더보드.

**고객지원 문서 'Third-Party Applications'의 요지** (2025-02-10 표기, https://support.riotgames.com/en-us/riot/events/third-party-applications , 확인일 2026-11-02)
- 금지 예시 네 가지: 의도적으로 가린 정보 노출 / 플레이어 대신 행동(봇·스크립트) / **실시간으로 플레이어 대신 게임 플레이 분석** / 보이는 것 변경(줌핵, 전역 궁극기 알림 등).
- Riot의 입장: 먼저 게임을 플레이하고, 분석과 반성은 그 뒤에 하라는 것. 안전한 앱 목록은 공개하지 않는다.
- 제재: 플레이 패턴·전적·신고를 보고 계정 단위로 판단한다. 기간 정지부터 영구 정지, 시즌 보상 회수까지 가능.

**GRIMOIRE에 적용한 해석** (조사원 해석, 최종 판단은 Riot DevRel 문의 권장 `[검증 필요]`)

| 기능 아이디어 | 판정 | 근거 |
|---|---|---|
| 게임 후 복기·타임스탬프 피드백·처방 레슨 | ✅ 허용 쪽 | 본인 경기 기록을 보는 훈련 도구가 승인 사례이고, '플레이 후 분석'이 Riot의 입장 |
| 챔피언 선택 전·중 정적 정보(매치업 레슨 링크, 상대 챔피언 교과서) | ✅ 허용 쪽(정적 데이터) | 게임 전 정적 데이터 오버레이 승인 |
| 챔피언 선택 중 "이 픽 하라" 단정 추천 | ⚠️ 주의 | "결정 지시 앱"·"다양성 감소" 금지. 복수 선택지 + 근거 형태로만 |
| 게임 중 실시간 코칭(음성·오버레이) | ❌ 피함 | 실시간 분석 금지 예시. STATUP.GG·YOUR.GG 오버레이는 회색지대 |
| 적 스펠·궁 쿨다운 타이머(수동 입력 포함) | ❌ 금지 | 정책이 "수동 추적"까지 명시. 2020년 Riot이 승인했다는 보도가 있던 음성 입력형 'Lola'(https://www.dexerto.fr/league-of-legends/riot-games-approuve-un-assistant-vocal-qui-vous-aide-a-mieux-jouer-a-lol-1277382/) 같은 사례는 현재 정책으로는 통하지 않음 |
| 적 정글 동선 추적 알림(레벨·CS 변화 기반) | ❌ 피함 | "그 세션에서 몰랐던 정보" 조항에 걸릴 소지. YOUR.GG가 2026-06 출시했지만 Riot 승인 여부는 `[확인 못 함]` |
| 자체 MMR·숨은 점수 추정 | ❌ 금지 | MMR 계산기 금지. "실력 지수"는 MMR처럼 보이지 않게 설계(예: 원자 개념 숙련도) |
| 상대 플레이어 개인 분석(로딩 화면 상대 성향) | ⚠️ 주의 | Porofessor 등이 해 왔지만 스트리머 모드처럼 숨긴 플레이어 분석은 금지 |

> 위 판정 중 ⚠️와 `[검증 필요]` 항목은 **I장 'Riot DevRel 질문 목록'**에 질문으로 모았다.

### B-5. 2025~2026 정책·보안 변화 타임라인 (설계에 직접 영향)

| 날짜 | 변화 | 출처 |
|---|---|---|
| 2024-04 | Vanguard 도입 FAQ: 외부 도구의 메모리 읽기가 막힐 수 있고 허용 목록은 없다고 안내 | https://hexgate.app/blog/riot-third-party-apps/ `[2차]` |
| 2025-02-10 | 서드파티 앱 고객지원 문서 표기일(실시간 분석 금지 등) | https://support.riotgames.com/en-us/riot/events/third-party-applications |
| 2025-05-29 | 개발자 일반 정책 갱신: Riot 자산(인게임 오버레이·로딩 화면·Riot 클라이언트) 내 광고 금지 | https://developer.riotgames.com/policies/general , https://eloking.com/blog/lol-riot-bans-overlay-advertisements `[2차]` |
| 2025-08 | OP.GG 스펠·스킬 트래커 오버레이 제거(정책 변경 사유) | https://hexgate.app/blog/riot-third-party-apps/ `[2차]` |
| **2026-09-29** | Riot 공지: **2026-10-06부터 알 수 없는 서드파티 앱의 게임 메모리 접근을 기본 차단**(LoL·TFT·VALORANT). 차단 자체로 제재하지는 않음. **기존 승인 앱은 2027-04까지 유예**, 그 뒤엔 공식 API로 옮겨야 함. 이유: "AI 보조 리버스 엔지니어링"으로 해킹이 급증했고, 현재 AI 모델에 의한 남용·계측을 막아야 함 | https://support.riotgames.com/en-us/riot/performance/game-memory-access-removed-for-third-party-apps `[공식]` |
| 2026-10-06~07 | 26.20 노트(초기): 리플레이를 **본인이 참여한 경기만** 받도록 비공개화 예고 → 크리에이터·코치 반발 | https://www.pcgamesn.com/league-of-legends/match-replays-private , https://gameriv.com/lol-creators-criticize-personal-match-replay-limits-as-riot-explains-security-concerns/ `[2차]` |
| 2026-10 | Riot이 비공개화를 26.21로 연기 | https://insider-gaming.com/riot-delays-private-league-of-legends-replays/ `[2차]` |
| **2026-10-20** | 26.21 노트: 비공개화 대신 **리플레이 다운로드 시간당 10회 제한**. 본인·친구·타인 리플레이는 계속 받을 수 있음. 이유: "vibecoded apps"를 쓰는 악성 행위자의 대규모 스크래핑 | https://www.leagueoflegends.com/en-us/news/game-updates/league-of-legends-patch-26-21-notes/ , https://support.riotgames.com/en-us/league-of-legends/gameplay/replays-faq-pro-tips `[공식]` |

- 해석: Riot의 보안 방향은 **"공식 API만, 대량 수집 억제, AI 자동화 경계"**다. 26.20 노트 초안에는 "서드파티 앱·사이트는 리플레이 다운로드 접근을 완전히 잃는다"는 보도가 있었으나, 현재 공식 FAQ 기준으로는 시간당 10회 제한만 적용된다. 앞으로 다시 조일 가능성은 열어 둬야 한다 `[검증 필요]`.
- 26.21 노트는 약속했던 크리에이터·코치용 해법을 담지 않았다는 2차 지적이 있다. https://senseos.gg/guides/league-of-legends/how-replays-work/

### B-6. 상표·제품명 규칙과 법적 고지

- **개발자 정책**: Riot 게임·제품과 스타일·기능이 비슷하면 안 된다. Riot 로고·상표(프레스 킷)는 제품의 핵심 가치를 위해 **불가피할 때만** 쓴다. 승인 자산은 Data Dragon, Press Kit 등. Riot IP로 게임을 만들면 안 된다. 제품명에 관한 명시 규칙은 개발자 정책에서 **찾지 못했다** `[확인 못 함]`.
- **팬 콘텐츠 정책('Legal Jibber Jabber')**: Riot 로고·상표는 서면 라이선스 없이 쓸 수 없다. **도메인명·SNS 계정·검색 키워드에 Riot 이름을 쓰지 말 것**(Riot과 관련 있다고 오인하지 않게). API 프로젝트는 유효한 키와 API 정책을 지키면 상업화할 수 있다. 앱스토어 출시에는 서면 라이선스 또는 유효한 API 키가 필요하다. https://www.riotgames.com/en/legal
- 업계 관행: lolalytics, leagueofgraphs처럼 도메인에 'lol'을 쓰는 서비스가 있지만 규정 위반 여부와 별개다. **'GRIMOIRE — 협곡의 마도서'처럼 Riot 상표를 뺀 이름이 안전하다.** '협곡'은 일반명사다. 'Summoner's Rift', 'League' 같은 표현은 제품명·도메인에서 빼기를 권한다 `[조사원 권고]`.
- **법적 고지(필수)**: 플레이어가 쉽게 볼 수 있는 곳에 표시해야 한다. 공식 정책의 Core Policies 절에 있는 문구를 그대로 복사해 쓴다. 구조는 다음과 같다: "[제품명]은 Riot Games의 보증을 받지 않았으며 Riot Games 또는 Riot Games 자산의 제작·관리에 공식 관여한 누구의 견해도 반영하지 않는다 + Riot Games와 관련 자산은 Riot Games, Inc.의 상표 또는 등록상표다." 원문 시작 구절: "isn't endorsed by Riot Games". 실제 적용 예: Mobalytics·OP.GG·YOUR.GG·DeepLoL 사이트 하단(모두 확인일 2026-11-02). https://developer.riotgames.com/policies/general
- 앱스토어 앱: 스토어 설명에도 같은 고지를 넣는 것이 업계 관행이다 `[조사원 권고]`.

### B-7. AI/LLM 관련 언급

- 개발자 일반 정책과 API 약관에는 AI·LLM·머신러닝·모델 학습에 관한 조항이 **없다** (확인일 2026-11-02). https://developer.riotgames.com/policies/general , https://developer.riotgames.com/terms
- Riot이 AI를 직접 언급한 문서: 2026-09-29 메모리 차단 공지("AI 보조 리버스 엔지니어링", "현재 AI 모델에 의한 남용·계측"), 26.21 노트("vibecoded apps"를 쓰는 악성 행위자의 대규모 스크래핑). → **AI 자체를 금지하지는 않지만, AI를 이용한 자동화·대량 수집·게임 계측을 적대적으로 본다.**
- 중개 금지: LoL 정책 페이지에 "Riot API와 다른 제3자 회사 사이의 data broker 역할 앱 금지" 취지의 문구가 있다는 검색 요약이 있다 `[2차, 원문 재확인 필요]`. Riftbound 정책에는 API 데이터를 제3자에게 넘기거나 파는 "middle-man" 사용 금지가 명시돼 있다. https://developer.riotgames.com/policies/riftbound
- **아키텍트 주의**: 외부 LLM API(Anthropic 등)에 경기 데이터를 보내 해설을 만드는 것은 '처리 위탁'이지 '재판매'는 아니라고 보는 게 일반적 해석이다. 그래도 제품 등록 양식에 **AI 사용 방식(외부 LLM에 비식별 경기 데이터 전송, 학습 미사용)을 명시**하고 DevRel 확인을 받기를 권한다 `[검증 필요]`.

### B-8. 데이터 보관·개인정보 조건

| 항목 | 내용 | 출처 |
|---|---|---|
| 보관 기한 | 일반 정책·API 약관에 **구체적인 저장·캐시 기한 없음** | https://developer.riotgames.com/terms , https://developer.riotgames.com/policies/general |
| 종료 시 삭제 | 라이선스가 끝나면 보유한 Game Information을 **전부 삭제** | API 약관 |
| 삭제 요청 전달 | Riot이 최종 사용자의 개인정보 삭제 요청을 식별자(accountId 등)와 함께 Riot 채널로 전달하고, 개발자는 이를 처리해야 함. Riot은 GDPR 기준을 전 세계에 적용한다고 명시 | API 약관 |
| 익명성 | 보이는 정보로 식별할 수 없는 플레이어의 익명성 해제 금지, 게임이 숨긴 플레이어 분석 금지 | 일반 정책, LoL 문서 |
| 본인 인증 | 사용자 설정 게임 전적은 RSO(Riot Sign On)로 본인에게만 제공. 계정 소유 확인에도 RSO 사용(`/riot/account/v1/accounts/me`) | https://developer.riotgames.com/docs/lol |
| 국내법 | 한국 개인정보 보호법(PIPA)·전자상거래법·정보통신망법·AI 기본법 등 → **H장 '법무 체크리스트'**로 정리(조사 결과이며 법률 자문 아님, 출시 전 변호사 검토 전제) | H장 |

- 시사점: 유저 경기 데이터는 **RSO 로그인 + 명시적 동의** 기반으로 수집하고, 삭제 요청 파이프라인(Riot 전달분 + 자체 탈퇴)을 처음부터 설계한다.

---

## C. 데이터 소스 사실 확인

### C-1. match-v5 + timeline

| 항목 | 사실 | 출처 |
|---|---|---|
| 라우팅 | KR 플랫폼 호스트 `kr.api.riotgames.com`. 리전 호스트 AMERICAS/ASIA/EUROPE/SEA. match-v5는 리전 라우팅을 쓴다(KR → ASIA가 통례 `[검증 필요]`: 포털은 "Reference 페이지에서 샘플 요청으로 확인"하라고만 함) | https://developer.riotgames.com/docs/lol |
| **프레임 간격** | `info.frameInterval = 60000` (ms) → **1분에 프레임 1개** | https://gist.github.com/RiotTuxedo/ab04ac027546f711ce820faed394295b , https://velog.io/@dongdongju96/RIOT-API-사용법-2 |
| 프레임 구조 | `frames[]` 각각에 `events[]` + `participantFrames{1..10}` + `timestamp` | 위와 같음, https://docs.rs/riven/latest/riven/models/match_v5/index.html |
| participantFrame 필드(12) | `championStats`, `currentGold`, `damageStats`, `goldPerSecond`, `jungleMinionsKilled`, `level`, `minionsKilled`, `participantId`, **`position{x,y}`**, `timeEnemySpentControlled`, `totalGold`, `xp` | https://docs.rs/riven/latest/riven/models/match_v5/struct.ParticipantFrame.html (riven 2.81.0, Riot 스펙에서 자동 생성되는 라이브러리) |
| 이벤트 공통 필드 | `timestamp`(ms), `type`, `participantId`, `killerId`, `victimId`, `assistingParticipantIds`, `position`, `itemId`, `skillSlot`, `levelUpType`, `wardType`, `creatorId`, `bounty`, `shutdownBounty`, `killStreakLength`, `multiKillLength`, `killType`, `laneType`, `teamId`, `killerTeamId`, `monsterType`, `monsterSubType`, `buildingType`, `towerType`, `beforeId/afterId`, `goldGain`, `winningTeam`, `transformType`, `featType/featValue`, **`victimDamageDealt/Received`**, `victimTeamfightDamageDealt/Received` 등 40개 | https://docs.rs/riven/latest/riven/models/match_v5/struct.EventsTimeLine.html |
| 이벤트 타입(확인) | `CHAMPION_KILL`, `CHAMPION_SPECIAL_KILL`, `ELITE_MONSTER_KILL`(killerTeamId 포함), `TURRET_PLATE_DESTROYED`(laneType·teamId·position), `LEVEL_UP`, `PAUSE_START`, `PAUSE_END`, `GAME_END`, `CHAMPION_TRANSFORM`, `DRAGON_SOUL_GIVEN` | https://gist.github.com/RiotTuxedo/758ee4d88693b768a880ece93cd78663 (Riot 직원 gist, 2022-02 마지막 활동) |
| 이벤트 타입(필드로 추정) | `ITEM_PURCHASED/SOLD/DESTROYED/UNDO`, `SKILL_LEVEL_UP`, `BUILDING_KILL`, `OBJECTIVE_BOUNTY_*`, `FEAT_UPDATE`(2025 위업용, 2026 제거) → 열거값 원문은 `[확인 못 함]` | 위 필드 목록에서 추정 |
| **와드 이벤트 위치 (확정)** | **`WARD_PLACED`·`WARD_KILL`에는 위치(`position`) 필드가 없다.** Riot 직원 샘플 타임라인에서 `WARD_PLACED`는 `creatorId·timestamp·type·wardType`, `WARD_KILL`은 `killerId·timestamp·type·wardType`만 있다(리뷰어가 `WARD_PLACED` 78건 전수 확인, 조사원이 `WARD_PLACED` 9건 재확인. `WARD_KILL` 필드는 리뷰어 확인값). 같은 샘플의 `ELITE_MONSTER_KILL`에는 `position`이 있다. → **"어디에 와드를 놓았나"는 match-v5로 알 수 없고**, 알 수 있는 것은 설치 시각·종류·설치자뿐이다. 바위게 처치는 별도 이벤트가 없다(샘플 기준 `ELITE_MONSTER_KILL`은 드래곤·바론만) | https://gist.github.com/RiotTuxedo/ab04ac027546f711ce820faed394295b (2022 샘플, 확인일 2026-11-03), review.md 1.4. 현 패치 실응답 1건으로 재확인 권장(엔진 담당, 키 필요) |
| 사망 피해 내역 | 킬 이벤트의 `victimDamageReceived`로 **죽기 직전 받은 피해의 출처(챔피언·스킬)**를 알 수 있다 → "누구의 어떤 스킬에 죽었나" 분석 가능 | riven 필드 + Riot gist |

**해상도 한계 (엔진 설계의 핵심 사실)**
- 위치는 **1분마다 1회** + 위치를 담은 이벤트(킬, 포탑 방패 파괴, 에픽 몬스터 처치 등)가 일어날 때만 있다. 평타·스킬 시전·체력 변화·이동 경로·카이팅·**와드 위치**는 **타임라인에 없다.**
- 따라서 "백무빙 1회 때문에 딜교 손해"는 **match-v5로는 측정할 수 없다.** match-v5로 가능한 해상도: CS·골드·XP의 분당 추이, 귀환·구매 타이밍(구매 이벤트로 추정), 와드 설치·제거의 **시각·종류·개수**(위치 없음), 킬·데스 위치와 피해 구성, 오브젝트 타이밍, 포탑 방패.
- → 미시 해상도는 C-3(리플레이) 경로가 필요하다. 상세 설계는 engine.md로.

### C-2. Live Client Data API (`https://127.0.0.1:2999/liveclientdata/`)

| 항목 | 사실 |
|---|---|
| 접근 | **로컬 네이티브 앱만**, 게임이 진행 중일 때만. 자체 서명 인증서(무시하거나 제공 루트 인증서 사용) |
| 엔드포인트 | `/allgamedata`, `/activeplayer`, `/activeplayername`, `/activeplayerabilities`, `/activeplayerrunes`, `/playerlist`, `/playerscores?riotId=`, `/playersummonerspells`, `/playermainrunes`, `/playeritems`, `/eventdata`, `/gamestats` |
| 본인(activeplayer) | `championStats`(공격력·주문력·방어력·마저·이동속도 등, **`currentHealth`·`maxHealth` 포함**), `currentGold`, 스킬, 전체 룬, 레벨. 현재 체력은 **본인만** 얻을 수 있다(공식 문서 샘플 응답, 확인일 2026-11-03) |
| 전체(playerlist) | 챔피언, 팀, 아이템, 레벨, 점수(K/D/A·CS·와드 점수), 소환사 주문, 메인 룬, `isDead`, `respawnTimer`, `position`(**좌표가 아니라 역할**, 예: "MIDDLE") |
| 좌표 | **제공하지 않음** |
| 이벤트 | `/eventdata`: 이벤트 ID·시간·이름(샘플 문서에는 `GameStart`만 표시) |
| 정책 위치 | 공식 경로라 메모리 차단 대상이 아님. 다만 게임 중에 분석을 보여 주면 '실시간 분석 금지'에 걸릴 소지가 있음 → **게임 중에는 기록만 하고 게임 후 보여 주는** 방식이 정책 해석상 안전한 쪽 `[검증 필요]` |

- 출처: https://developer.riotgames.com/docs/lol , https://hexgate.app/blog/riot-third-party-apps/ `[2차]` (확인일 2026-11-02)
- 쓸모: 1초 단위 폴링이면 **본인의 체력·골드·아이템·레벨 변화와 이벤트 시각**을 1분 프레임보다 촘촘하게 남길 수 있다(타임라인 보강용). 상대 체력·좌표는 얻을 수 없다.

### C-3. Replay API (게임 클라이언트 로컬)

| 항목 | 사실 |
|---|---|
| 활성화 | `game.cfg`(예: `C:\Riot Games\League of Legends\Config\game.cfg`) `[General]`에 `EnableReplayApi=1`. **기본 비활성.** 켜면 Swagger v2/OpenAPI v3 스펙이 생성됨 |
| 엔드포인트 | `GET /replay/game`(클라이언트 프로세스 정보) · `GET/POST /replay/playback`(재생/일시정지, 시간 이동 등) · `GET/POST /replay/render`(카메라·렌더 속성) · `GET/POST /replay/recording`(코덱·출력 경로를 지정해 **녹화 시작**, GET으로 진행 확인) · `GET/POST /replay/sequence`(키프레임 시퀀스 적용) |
| 참고 구현 | Riot 공식 오픈소스 **League Director**(Apache-2.0, **Windows 전용**): 1인칭 카메라, 챔피언·미니언에 카메라 고정, 재생 속도, 키프레임 시퀀서, **WebM/PNG 캡처**, HUD·체력바·알림 표시 전환, FOV·안개·피사계심도 등. 그래픽 '매우 높음' 필요. PR은 받지 않음 |
| 데이터 | Replay API는 **카메라·재생 제어용이지 경기 데이터 API가 아니다.** 좌표·체력은 화면 인식(CV)으로 뽑아야 함. 리플레이 재생 중 Live Client Data API 동작 여부 `[검증 필요]` |
| 리플레이 수명 | **패치가 바뀌면 만료**(재생 불가), 클라이언트에는 **최근 20판**만 |
| 다운로드 제한 | 26.21부터 **시간당 10회**(본인·타인 공통) |

- 출처: https://developer.riotgames.com/docs/lol , https://github.com/RiotGames/leaguedirector , https://support.riotgames.com/en-us/league-of-legends/gameplay/replays-faq-pro-tips (2026-03-27 표기, 확인일 2026-11-02)
- **설계 함의**: 데스크톱 컴패니언이 (1) 사용자 본인 경기 리플레이를 받고 → (2) **같은 패치 안에(약 2주)** Replay API로 재생·녹화하고 → (3) 프레임을 CV로 분석하는 흐름이 미시 피드백의 유일한 공식 경로다. 패치가 넘어가면 분석할 수 없으므로 **경기 직후 자동 처리 큐**가 필요하다. 사용자 PC 렌더링 부하·시간, Windows 전용, Mac 미지원은 → engine.md 검토.

### C-4. LCU API (League Client API)

- 로컬 CEF↔C++ 통신이며 **"서드파티 앱용으로 공식 지원하지 않는다."** 문서화·가동률·변경 공지를 보장하지 않는다. 쓰려면 포털에서 앱을 새로 만들거나 기존 앱에 사용 메모를 남겨 **등록**하라고 한다. https://developer.riotgames.com/docs/lol (확인일 2026-11-02)
- 메모리 차단 공지 이후에도 LCU·Riot API·Live Client Data API를 쓰는 앱은 차단 대상이 아니라는 2차 정리가 있다. https://hexgate.app/blog/riot-third-party-apps/ `[2차]`
- 용도 예: 로그인 계정 확인, 챔피언 선택 상태, 리플레이 다운로드 트리거 등. 구체 엔드포인트는 비공식이라 `[검증 필요]`. **핵심 기능을 LCU에 걸지 말 것.**

### C-5. .rofl 리플레이 파일

- 공식 개발자 문서에는 .rofl 포맷 설명이 **없다** `[확인 못 함]`.
- 커뮤니티 분석: 헤더 + JSON 메타데이터(경기 길이, 게임 버전, 플레이어 통계) + **Blowfish로 암호화된 청크 페이로드**. 메타데이터는 쉽게 읽히지만 경기 내용 복호화는 별도 작업이다. https://gist.github.com/neko-neko-nyan/82b6119a23eb65cc5b0222bb8448310b , https://docs.rs/crate/lolrofl/latest `[2차]`
- 비공식 파서(lolrofl)는 "하위 호환을 기대하지 말 것"이라고 명시한다. 와드 위치처럼 API가 주지 않는 데이터를 뽑으려던 프로젝트(ROFL)는 보관(archived) 상태다. https://github.com/Mowokuma/ROFL
- **재생은 같은 패치에서만 된다**(Riot FAQ). → .rofl 직접 해독은 정책 리스크(리버스 엔지니어링)와 유지보수 리스크가 커서 **비권장**. 공식 Replay API 재생 + 화면 캡처 경로를 권장한다.

### C-6. 프로 경기 데이터

| 소스 | 사실 | 출처 |
|---|---|---|
| **GRID** (Riot 독점 데이터 배포 파트너) | 2023년 3년 계약으로 Bayes를 대체했고, 2024-25 시즌부터 공식 라이브 데이터 제공. Riot이 GRID 지분 보유 | https://grid.gg/riot-games-and-grid-announce-exclusive-global-esports-data-partnership/ , https://www.sportico.com/business/tech/2023/riot-games-grid-data-deal-1234748582/ |
| 접근 조건 | 라이브·일정·A/V 데이터는 **"GRID를 통한 상업 목적에만"**. **LCK 데이터는 "미디어 목적 한정"**. 비상업 사용자(연구자·커뮤니티)는 현재 지원 안 함, 비상업 데이터 포털은 계획만 있음. 무료 등급 표시 없음 | https://riotesportsdata.com/league-of-legends (확인일 2026-11-02) |
| 데이터 항목 | 체력·마나, 골드, 아이템, 리스폰 타이머, **선수 위치**, 오브젝트 상태, CS, K/D/A, 밴픽(순서 포함), 골드·XP 차이, 승리 확률 피드 | 위와 같음, https://grid.gg/data/games/league-of-legends/ |
| 가격 | 공개 안 함(영업 문의), 계약 범위·지역·기간 내 라이선스 | https://grid.gg/data/games/league-of-legends/ |
| 한국 사례 | 2026-04-08 NAVER(치지직) × GRID 공식 실시간 LoL e스포츠 데이터 제휴 보도. LCK 포함 여부 `[확인 못 함]` | 검색 요약 `[2차]` https://grid.gg/newsroom/ |
| LoL Esports Data Portal(구 Bayes) | 2022-06: 팀·파트너와 Leaguepedia·Oracle's Elixir 같은 커뮤니티 프로젝트에 매치 히스토리·리플레이·텔레메트리 제공. 커뮤니티 개방은 "후속 단계"로 예고 | https://lolesports.com/en-US/news/dev-diary-introducing-the-new-lol-esports-data-portal |
| 커뮤니티 데이터 | Oracle's Elixir: 2014년~ 프로 경기 CSV(연도별). Leaguepedia API. 2026년 갱신 여부·라이선스 `[확인 못 함]` | https://github.com/FloPrm/lol_analytics , https://github.com/jadenoca/LolEsportsData `[2차]` |

- 시사점: 프로 데이터의 **위치·타임라인 수준 원본은 상업 계약(GRID) 없이는 얻을 수 없다.** MVP의 '정석 엔진'은 **KR 챌린저·그랜드마스터 솔로랭크(공식 API)**를 주 표본으로 쓰고, 프로 데이터는 밴픽·오브젝트 타이밍 같은 집계 수준(커뮤니티 CSV, 라이선스 확인 후)으로 보조한다.

### C-7. 프로 경기 영상 저작권·사용 범위

- 팬 콘텐츠 정책은 원본성을 요구한다: 기존 콘텐츠를 베끼거나 가벼운 해설만 붙이지 말라고 하며, 예시로 **e스포츠 경기와 다른 플레이어의 VOD**를 든다. Riot은 자동 도구로 이를 탐지할 수 있다. 페이월 콘텐츠는 서면 라이선스 없이 금지(API 프로젝트 예외). https://www.riotgames.com/en/legal (확인일 2026-11-02)
- LCK 중계 영상 권리(LCK·중계사)의 세부 이용 조건은 `[확인 못 함]`.
- **권고**: (1) 프로 VOD를 유료 앱에 재호스팅하지 않는다. (2) 공식 채널 영상을 링크·임베드(유튜브 임베드 약관 준수 `[검증 필요]`)하고 타임스탬프만 붙인다. (3) 장면 해설은 **우리가 데이터로 다시 그린 2D 미니맵 재구성 애니메이션**으로 대체한다.

### C-8. 챌린저 게임 대량 수집 — 정책 범위 안의 현실적 방법

1. `league-v4` 상위 리그 엔드포인트(Challenger/Grandmaster/Master)로 KR 상위 플레이어 PUUID 목록 확보 → 2. `match-v5` by-puuid로 솔로랭크(queue 420) 경기 ID 수집(중복 제거) → 3. 경기 상세 + 타임라인 수집 → 4. 패치 태그(`gameVersion`)를 붙여 저장.
- 용량 추정 `[계산]`: Personal 키(2분 100회) = 하루 최대 72,000회 → 경기 1개에 2회(상세+타임라인)라면 **하루 최대 약 36,000경기**(다른 호출 0 가정, 메서드 리밋 무시). Production 시작값(10분 30,000회) = 하루 최대 4,320,000회 → 약 216만 경기(메서드 리밋 미확인이라 실제로는 더 적음 `[검증 필요]`).
- 규모 참고: LoLalytics는 26.21(16.21) 패치 에메랄드+ 표본을 챔피언 **24,575,060개**(26.20은 27,114,869개)로 표시한다. 한 경기에 챔피언 10개이므로 약 246만 경기에 해당 `[계산]`. https://lolalytics.com/ (확인일 2026-11-02)
- **KR 상위 티어 정원 `[공식]`**(Riot 고객지원 'Master, Grandmaster, and Challenger: The Apex Tiers', 2026-05-30 게시, 확인일 2026-11-03):

  | 큐 | 그랜드마스터 | 챌린저 | 마스터 | 진입 LP 하한 |
  |---|---|---|---|---|
  | 솔로/듀오 (KR) | **700명** | **300명** | **정원 없음** | GM 400 LP · 챌린저 800 LP 이상이면서 해당 티어 최하위보다 LP가 높아야 함 |
  | 자유 랭크 (KR) | 500명 | 200명 | 정원 없음 | 같음 |

  https://support.riotgames.com/en-us/league-of-legends/gameplay/master-grandmaster-and-challenger-the-apex-tiers
- → 정원이 꽉 찬 경우 **KR 솔로랭크 GM+ = 최대 1,000명**이다. **마스터 인원(따라서 마스터+ 전체 인원)은 정원이 없어 공개 웹 자료로는 확인하지 못했다** `[확인 못 함]`. 리뷰어가 제안한 `league-v4` 응답 수 집계는 API 키가 필요해 이번 조사에서 하지 않았다(안전 규칙). 엔진 담당이 키 발급 후 `league-v4` 마스터 리그 응답의 `entries` 수로 확인하는 것이 가장 정확하다. 전적 사이트의 티어 분포 페이지(leagueofgraphs)는 403으로 열리지 않았다.
- 기여자 규모 가정(engine 6.2 '마스터+ 1,000~3,000명')과 비교하면: GM+ 정원 1,000명 **전원이 동의해도** 1,000명이다. 3,000명은 마스터 인원이 2,000명 이상이고 마스터+ **대부분이 동의**해야 나오는 수치다 `[계산]`. 비율 가정은 보수적으로 잡아야 한다.
- 금지·비권장: 리플레이 대량 다운로드(시간당 10회 제한, "대규모 스크래핑" 경계), 메모리 읽기, .rofl 해독, 숨긴 플레이어 분석, 키 여러 개로 리밋 우회(키 하나에 제품 하나 원칙).
- 공개 서비스는 Production 키가 필요하고, Production 키 심사에는 작동하는 프로토타입이 필요하다 → **개발 단계 데이터 파이프라인은 Personal 키 한도 안에서 설계·검증**한다.

---

## D. 현재(2026-11) 게임 상태 사실 확인

### D-1. 패치 번호 체계와 최신 패치

| 항목 | 사실 | 출처 (확인일) |
|---|---|---|
| 번호 체계 | 2026년 패치는 **26.01~26.xx**(연도 + 순번). Data Dragon·통계 사이트는 **16.xx.1**로 표기(예: 26.21 = 16.21.1) | https://ddragon.leagueoflegends.com/api/versions.json (최신 "16.21.1"), https://lolalytics.com/ (2026-11-02) |
| 최신 패치 | **26.21** (공식 노트 2026-10-20 게시, 위키 기준 배포 2026-10-21) | https://www.leagueoflegends.com/en-us/news/tags/patch-notes/ , https://wiki.leagueoflegends.com/en-us/Patch/2026_Annual_Cycle (2026-11-02) |
| 다음 패치 | 26.22 — 2026-11-04 예정 `[2차]` | https://senseos.gg/guides/league-of-legends/how-replays-work/ (2026-11-02) |
| 주기 | 대체로 2주(26.01 1/8 → 26.21 10/21, 21개 패치) | 연간 사이클 위키 |
| 2026 연간 구조 | **시즌 1**(26.01, 1/8~, 데마시아 테마) → **시즌 2**(26.09, 4/29~, Pandemonium) → **시즌 3**(26.15, 7/29~, 'Classic' 테마) | 연간 사이클 위키, https://wiki.leagueoflegends.com/en-us/V26.09 |
| 'League Classic' 모드 | 26.15(2026-07-29) 출시. 2013년 시즌3 무렵의 옛 리그를 재현한 **별도 피처드 모드**(챔피언 60명, 옛 키트·아이템, 독립 밸런스) → **분석 대상에서 큐 ID로 제외해야 함** `[2차]` | https://blog.loltheory.gg/league-of-legends-classic/ , https://www.gamedeveloper.com/press-release/league-of-legends-classic-releases-in-patch-26-15 |

### D-2. 2026 시즌 주요 시스템 변화 요약

| 패치 | 변화 | 교과서 영향 | 출처 |
|---|---|---|---|
| 26.01 | **역할 퀘스트 5개 포지션 전체 도입**(기존 정글·서폿 퀘스트 확장) | 라인전 "왜 라인에 있어야 하나"의 보상 구조가 바뀜 | https://wiki.leagueoflegends.com/en-us/Role_Quest |
| 26.01 | **아타칸 제거**, **힘의 위업(Feats of Strength) 제거** | 2025 자료 폐기 필요 | https://wiki.leagueoflegends.com/en-us/V26.01 |
| 26.01 | **게임 템포 가속**: 첫 미니언 1:05→**0:30**, 정글 몬스터 1:30→**0:55**, 패시브 골드 1:40→**1:05**, 바론 25:00→**20:00**, 미니언 이동속도 +25 | 모든 타이밍 레슨의 수치가 2025와 다름 | V26.01 |
| 26.01 | **포탑 방패가 14:00에 사라지지 않음**(골드 125→120, 방패 체력 구간 10/25/45/70/100%), 2·3차 포탑에도 방패 추가, 외곽 포탑 체력 5000→9000, 첫 포탑 +300골드, 포탑이 근접 챔피언에게 받는 피해 +20% | "14분 방패 소멸 전 압박" 개념 폐기, 스플릿·포탑 압박 레슨 재작성 | V26.01, https://wiki.leagueoflegends.com/en-us/Turret |
| 26.01 | **결정질 과성장(Crystalline Overgrowth)** 신설: 라인 포탑에 일정 시간 뒤 수정이 자라고, 다음 적 챔피언 공격이 최대 체력 비례 고정 피해 | 초반 포탑 견제·다이브 타이밍 | V26.01, Turret 페이지 |
| 26.01 | **요정빛(Faelights)** 신설: 지정 와드 위치에 와드를 놓으면 시야 반경 +25%와 추가 시야 구역 45초 | 와딩 레슨의 핵심 지점이 바뀜 | V26.01 |
| 26.01 | 귀환 후 귀환 신속(Homeguard) 고정 시간 폐지, 구간형으로 변경 | 라인 복귀 시간 계산 | V26.01 |
| 26.01 | 치명타 기본 피해 175%→**200%**, 선취점 +100골드 부활, 솔로 미니언 경험치 95→100% | 원딜 성장·초반 킬 가치 | V26.01, https://wiki.leagueoflegends.com/en-us/Champion_gold_bounties |
| 26.01 | 정글 펫: 진화 간식 20/40→**15/35**, 블러드 로즈 제거, 정찰 꽃 리스폰 단축 | 정글 동선·파워 스파이크 | V26.01, https://wiki.leagueoflegends.com/en-us/Jungling |
| 26.01 | 용·전령·유충·바론 체력·방어 레벨 스케일 재설계, 고대의 원한(Ancient Grudge) 스택당 피해감소 7→15%(최대 60%) | 오브젝트 처치 시간·스틸 계산 | V26.01, https://wiki.leagueoflegends.com/en-us/Dragon |
| 26.09 | 역할 퀘스트 조정: 라인 밖 오브젝트 페널티 −75%(진행도에 따라 0%로 감소), 귀환 후 12초 패시브 포인트 정지, 라인 2분당 로밍 1분 면제 | 로밍 판단 규칙의 비용 함수 | https://wiki.leagueoflegends.com/en-us/V26.09 |
| 26.09 | **WASD 이동이 랭크에서 허용**됨. Riot은 WASD와 클릭 이동의 승률 차이가 통계적으로 유의하지 않다고 발표 | '무빙' 레슨은 두 조작계 모두 고려해야 함 | https://leagueoflegends.com/en-us/news/dev/dev-wasds-ranked-release , https://www.invenglobal.com/articles/20934/lol-to-introduce-wasd-controls-to-ranked-play-starting-with-patch-269 |
| 26.09 | 신규 시작 아이템(도란의 활·도란의 투구), 신규 핵심 룬(Deathfire Touch, Stormraider's Surge — 신속(Phase Rush) 대체·삭제), 칼날비·콩 너프 | 룬·시작템 레슨 | V26.09 |
| 26.09 | 피해 증가 효과가 곱연산→**합연산**으로 변경 | 딜 계산식 | V26.09 |
| 26.09 | 시즌 2: 마스터+ LP 기본 ±30, 일부 서버 마스터 하드 리셋(**KR은 목록에 없음**) | KR 상위 표본 연속성 | V26.09 |
| 26.10 | **미니언 어그로 변경**: 적 챔피언이 아군 미니언을 공격해도 그 미니언들의 어그로 우선순위 목록에 **더 이상 오르지 않는다**(미니언이 무작위로 대상을 바꾸는 느낌을 없애려는 조치). 노트가 없앤 것은 이 항목 하나다. 리뷰어 판단으로는 우선순위 표의 나머지 항목(예: 칠 미니언이 없으면 가장 가까운 적 챔피언)은 그대로다(review.md R-18) | 라인전 어그로·당기기·몸으로 받기 레슨. **적 미니언을 치는 것만으로는 미니언 어그로가 생기지 않는다**(26.09까지는 생길 수 있었음). 플레이어 행동으로 생기는 어그로는 적 챔피언을 공격했을 때의 지원 요청이 중심이 된다 | https://www.leagueoflegends.com/en-us/news/game-updates/league-of-legends-patch-26-10-notes/ (공식 노트, 리뷰어 확인), https://wiki.leagueoflegends.com/en-us/V26.10 (게임 → Minion Aggro, 배포 2026-05-13, 확인일 2026-11-03) |
| 26.15 | 시즌 3 시작, League Classic 모드 | 큐 필터 | 위 D-1 |
| 26.20 | 스코어보드에서 15개 아이템 스택 카운터 숨김("전략적으로 의미 있는 정보만") | 스코어보드 정보 범위 | https://www.leagueoflegends.com/en-us/news/game-updates/league-of-legends-patch-26-20-notes/ |
| 26.21 | 리플레이 다운로드 시간당 10회, 맵·오브젝트 변화 없음 | — | https://www.leagueoflegends.com/en-us/news/game-updates/league-of-legends-patch-26-21-notes/ |

> **'정글 펫'**은 2026에도 그대로다(정글 아이템으로 펫 소환, 대형 몬스터 처치 시 간식). 정글 역할 퀘스트 = 간식 35개 → 원시 강타.

### D-3. 현재 맵·오브젝트·경제 사실 시트 (교과서 설계자용)

> 값은 LoL 위키 기준이고, 대부분 26.01 변경이 반영돼 있다. 페이지마다 마지막 패치 표기가 달라 **26.21 시점 값은 모두 `[패치 26.21에서 재검증 필요]`**로 본다. 수치에는 `@26.xx`처럼 확인된 변경 패치를 태그로 단다.

**① 오브젝트**

| 대상 | 첫 등장 | 리스폰/소멸 | 보상·핵심 효과 | 태그·출처 |
|---|---|---|---|---|
| 원소 드래곤 | **5:00** | 처치 후 **5분** | 처치 골드 75, 지역 XP 160~400(레벨), 용 처치자 버프. **4번째** 원소 드래곤 = 드래곤 영혼. 2번째 처치 후 3번째 속성 결정과 함께 원소 협곡 변환 | @26.01 보상 변경 · https://wiki.leagueoflegends.com/en-us/Dragon · **첫 등장 5:00·리스폰 5분·장로 6분은 위키 Dragon pit 본문으로 확정**(패치 태그 없음, 확인일 2026-11-03) https://wiki.leagueoflegends.com/en-us/Dragon_pit |
| 장로 드래곤 | 한 팀이 원소 드래곤 4마리를 잡은 뒤 | **6분** | 글로벌 골드 150, 글로벌 XP 650, 시작 레벨 13 | @26.01 · Dragon |
| 공허 유충 | **8:00** | **게임당 1회**(리스폰 없음), 14:45 소멸(전투 중 14:55) | 3마리, 마리당 30골드·65XP·CS 2, 팀 전체 '공허의 손길' 스택 → 3스택이면 '공허의 굶주림'(아군 공허 진드기 소환으로 포탑 공략). 첫 마리만 에픽 처치로 집계 | @25.09 시간 · @26.01 보상 · https://wiki.leagueoflegends.com/en-us/Voidgrubs |
| 협곡의 전령 | **15:00** | 게임당 1회, 19:45 소멸(전투 중 19:55) | 100골드·240XP·CS 4. '전령의 눈'을 20초 안에 주워 소환(채널 1초) | @25.09 · @26.01 · https://wiki.leagueoflegends.com/en-us/Rift_Herald |
| 내셔 남작 | **20:00** (기존 25:00) | 처치 후 **6분** | 처치 골드 100 + 글로벌 150, 글로벌 XP 650, '내셔 남작의 손길' 180초(공격력·주문력, 강화 귀환, 주변 미니언 강화). 지는 팀에 레벨 차이 보정 XP | @26.01 · https://wiki.leagueoflegends.com/en-us/Baron_Nashor |
| 아타칸 | — | **제거됨** | — | @26.01 · V26.01 |
| 바위게(협곡 바위게) | **2:55** | **2:30** | 55~121골드, 100~150XP, 신속의 성소 90초 | @26.01 · https://wiki.leagueoflegends.com/en-us/Rift_Scuttler |

**② 정글 캠프**

| 캠프 | 첫 등장 | 리스폰 | 골드 / XP | 태그·출처 |
|---|---|---|---|---|
| 푸른 파수꾼(블루) | **0:55** | 5:00 | 90 / 95~142.5 | @26.01 · https://wiki.leagueoflegends.com/en-us/Blue_Sentinel |
| 붉은 덩굴정령(레드) | **0:55** | 5:00 | 90 / 95~142.5 | @26.01 · https://wiki.leagueoflegends.com/en-us/Red_Brambleback |
| 칼날부리 (붉은 칼날부리 1 + 작은 칼날부리 5) | **0:55** | 2:15 | 35 + 8×5 = **75** / 20 + 10×5 = **70**(레벨 1, 큰 놈 20~30·작은 놈 10~15로 레벨 비례) | @26.01 첫 스폰 · @11.10 리스폰 · @13.5 골드 · @14.10 XP · https://wiki.leagueoflegends.com/en-us/Raptor (조사원 재확인 2026-11-03) |
| 어스름 늑대 (큰 늑대 1 + 작은 늑대 2) | **0:55** | 2:15 | 55 + 15×2 = **85** / 50 + 15×2 = **80**(레벨 1) | @26.01 · https://wiki.leagueoflegends.com/en-us/Murk_Wolf |
| 돌거북(크룩) (고대 돌거북 1 → 미니 4, 돌거북 1 → 미니 2) | **1:07** | 2:15 | 15 + 10 + 14×6 = **109** / 15 + 10 + 16×6 = **121**(레벨 1) | @26.01 · https://wiki.leagueoflegends.com/en-us/Ancient_Krug |

> 레드·늑대·크룩 값은 curriculum 세션이 위키 개별 페이지에서 확인한 값(공유 노트 `curriculum-verified-facts.md`, 2026-11-02~03)을 옮겼고, 칼날부리는 조사원이 같은 위키 페이지로 재확인했다. XP는 레벨 1 기준이며 몬스터 레벨에 비례해 오른다.
| 심술두꺼비(그롬프) | **1:07** | 2:15 | 80 / 120~180 | @26.01 · https://wiki.leagueoflegends.com/en-us/Gromp |
| 정글 펫 진화 | 간식 15개(1차) / 35개(최종, 원시 강타) | — | 정글 퀘스트 = 간식 35개 | @26.01 · Jungling, Role_Quest |

**③ 미니언·경제**

| 항목 | 값 | 태그·출처 |
|---|---|---|
| 첫 웨이브 | **0:30** (기존 1:05) | @26.01 · https://wiki.leagueoflegends.com/en-us/Minion_(League_of_Legends) |
| 웨이브 간격 | 30초 → 14:00부터 25초 → 30:00부터 20초 | @26.01 |
| 구성 | 근접 3 + 원거리 3(+공성). 14:00 이후 공성 있는 웨이브는 근접 −1, 30:00 이후 원거리 −1 | @26.01 |
| 공성(대포) 미니언 | 첫 등장 **3번째 웨이브(1:30)**. 14:00까지 3웨이브마다, 14:00~25:00 2웨이브마다, 25:00 이후 매 웨이브 | @26.01 |
| 미니언 골드 | 근접 20, 원거리 14, 공성 50(0:30)→59(14:00)→66(25:00)→69(30:00) | 위키 웨이브 골드 표 |
| 미니언 경험치 | 솔로 100%(기존 95%), 공유 130%(기존 124%) | @26.01 · V26.01 |
| 미니언 이동속도 | 350/375/400/425/450(분 구간별, 구간 경계 `[확인 못 함]`) | @26.01 |
| 미니언 → 챔피언 피해 배율 | **55%**(25.S1.1에서 50→55). 26.01의 60%는 **구조물**에 주는 피해 | @25.S1.1 · https://www.riftpatchnotes.com/lol/system/minions (review.md 1.4 리뷰어 확인) |
| 미니언 기본 XP | 근접 62 · 원거리 31 · 공성 75, 경험치 공유 범위 1,500 | @26.01 · @25.S1.1(범위) · 같은 출처(curriculum 공유 노트·리뷰어 확인) |
| 레벨 XP 요구량 | 2레벨 280, 3레벨 +380(누적 660), 4레벨 +480(1,140), 5레벨 +580(1,720), 6레벨 +680(2,400) | https://wiki.leagueoflegends.com/en-us/Experience_(champion) (curriculum 공유 노트) |
| 시작 골드 | 500 | https://wiki.leagueoflegends.com/en-us/Gold |
| 패시브 골드 | 10초당 20.4, **1:05**부터 | Gold 페이지, @26.01 |
| 챔피언 처치 | 레벨 1~6 300골드, 레벨당 +10, 18레벨 420. **선취점 +100** | @25.09 스케일 · @26.01 선취점 · Champion_gold_bounties |
| 어시스트 | 처치 현상금의 절반(기본액 50% 상한)을 균등 분배, 초반엔 시간에 따라 50~100%로 감소 | 같은 출처 |

**④ 포탑**

| 항목 | 값 | 태그·출처 |
|---|---|---|
| 체력 | 외곽 9,000 / 내부 5,000 / 억제기 4,750 / 넥서스 3,500(파괴 3분 뒤 최대 체력 40%로 부활) | @26.01 · https://wiki.leagueoflegends.com/en-us/Turret |
| 포탑 방패 | 라인 포탑에 **5개**, 개당 **120골드**(지역), **14:00에 사라지지 않음**. 26.01부터 내부·억제기 포탑에도 방패 | @26.01 |
| 포탑 골드 | 글로벌: 외곽·넥서스 50, 내부·억제기 25. 첫 포탑 +300 | Turret 페이지 · @26.01 |
| 결정질 과성장 | **90초 뒤 = 게임 시각 1:40에 활성**(두 값은 같은 시점. Turret 페이지 본문이 "90초 뒤(게임 시각 1:40)"로 함께 적는다. 90초의 기준점은 본문에 없음). 그 시각에 포탑 근처에 적이 있으면 적이 없어질 때까지 미뤄진다. 레벨 1 최대 체력 2~3.3%, 18레벨 8.8~18.9% 고정 피해, 60초 최소 유지 → 240초 선형 증가 | @26.01 · https://wiki.leagueoflegends.com/en-us/Turret (확인일 2026-11-03) |
| 사거리·대상 전환 | 공격 사거리 **750**, 시야 1,350. 챔피언을 치고 있지 않을 때 **1,400 안에서 아군 챔피언에게 피해를 준 적 챔피언**으로 대상 전환(피해 없는 효과는 제외. 주문 보호막·무적에 막힌 피해, 회피·빗나감, 피해 0도 전환을 부름 — 2026-11-04 원문 재확인으로 정정) | Turret (확인일 2026-11-04) |
| 포탑 아래 막타 | 다른 피해가 없을 때 기본 공격 기준: **근접 미니언 = 포탑 2방 → 평타 1번**, **원거리 미니언 = 평타 → 포탑 1방 → 평타** | Turret (확인일 2026-11-03) |
| 불워크(Bulwark) | 체력 90/75/55/30%에서 20초 스택, 주변 적 챔피언 수에 비례해 방어·마저 증가 | Turret |
| 억제기 재생성 시간 | `[확인 못 함]` | — |

**⑤ 사망 타이머** (위키 기준, 26.x 변경 여부 `[검증 필요]`)
- 레벨별 기본 대기: 1~2레벨 10초, 3~4레벨 12초, 5레벨 14초, 6레벨 16초, 7레벨 20초, 8레벨 25초, 9레벨 28초, 10레벨 32.5초, 이후 레벨당 +2.5초 → 18레벨 52.5초. https://wiki.leagueoflegends.com/en-us/Death
- 시간 증가 계수: 15:00 전에는 없음. 15~30분 30초마다 0.425%, 30~45분 0.30%, 45~55분 1.45%, 최대 50%. 총 사망 시간 = 기본 × (1 + 계수).

**⑥ 역할 퀘스트 (현재값, 위키 최종 패치 표기 V26.19, 확인일 2026-11-02)**

| 역할 | 완료 조건 | 포인트 획득 | 보상 |
|---|---|---|---|
| 탑 | 1,200점 | 챔피언 처치 관여 15, 라인 미니언 2, 라인 포탑 50, 포탑 방패 40, 에픽 몬스터 30, 패시브(1:05부터, 라인에서 증가) | **해방된 순간이동**(역할 퀘스트 칸, 쿨 390초. 순간이동을 들었으면 최대 체력 35% 보호막으로 바뀌고 쿨 −30초), 600XP, 모든 XP +11%, 처치 관여당 +80XP, **레벨 상한 20** |
| 정글 | 35점 | 펫에게 준 간식 1개 = 1점 | 원시 강타, 캠프당 +10골드·+10XP, 정글·강에서 이동속도 4%(비전투 8%) |
| 미드 | 1,350점 | 처치 관여 25, 라인 미니언 2, 포탑 50, 방패 40, 에픽 30, 패시브, 챔피언 피해(근접 3%·원거리 1.5%) | 공격력·주문력 +8%(26.11), 2단계 신발 → 전용 3단계 신발 무료 업그레이드 |
| 바텀 | 1,350점 | 처치 관여 15, 라인 미니언 3, 포탑 50, 방패 40, 에픽 30, 패시브 | 300골드, 미니언당 +2골드, 처치 관여당 +40골드, 신발 전용 칸 |
| 서포터 | 800점 | 서포터 아이템 골드 1:1 등 | 세계 지도책 → 세계의 풍요 업그레이드, 제어 와드 40골드, 제어 와드 전용 칸 |
| 공통(26.09~) | 라인 밖 오브젝트 −75% 페널티(진행도에 따라 0%로 감소), 귀환 후 12초 패시브 정지, 라인 2분당 로밍 1분 면제 | | |

출처: https://wiki.leagueoflegends.com/en-us/Role_Quest , https://wiki.leagueoflegends.com/en-us/V26.09 (확인일 2026-11-02)

**⑦ 확인 못 한 항목(교과서 설계자 주의)** — 3단계 개정에서 해결된 항목은 취소선 대신 '해결'로 표시
- 해결: 레드·칼날부리·늑대·크룩 리스폰·골드·XP(② 표), 원소 드래곤 첫 등장 5:00(① 표), 결정질 과성장 90초 = 1:40(④ 표), 레벨별 경험치 요구량(③ 표)
- 남음: 드래곤 영혼 종류별 효과 현재값, 억제기 재생성 시간, 미니언 이동속도 구간 경계, 협곡의 전령 소멸 19:45(위키 본문과 패치 이력이 엇갈린다는 리뷰어 지적 → `[검증 필요]` 유지)
- 남음: 데마시아 테마 맵의 지형 변화 유무(위키는 "Demacia-themed map"이라고만 표기)

### D-4. 연습 모드(Practice Tool) 기능 범위 — 드릴 설계 전제

LoL 위키 Practice Tool 페이지 기준(최신 이력 V26.19, 확인일 2026-11-03). Riot 공식 문서가 아니라 위키 정리이므로 `[2차]`. https://wiki.leagueoflegends.com/en-us/Practice_Tool

| 분류 | 할 수 있는 것 |
|---|---|
| 자원 | 쿨다운·체력·마나 자동 회복, 골드 +10,000, 레벨 업, **경험치 레벨 고정** |
| 위치·생사 | 커서 위치로 순간이동, 부활, 내 챔피언 처치 |
| 포탑 | 포탑 무적 전환, 포탑 공격 끄기·켜기 |
| 미니언 | **미니언 생성 끄기·켜기**(끄면 기존 미니언도 사라짐). 웨이브 구성·위치를 직접 놓는 기능은 위키에 없음 |
| 시간 | 30초·10분 빨리 감기, **게임 속도 0.25·0.5·1·2배** |
| 허수아비 | 적·아군 허수아비 각 최대 4개, 최대 체력 ±100·저항 ±10 조정, 대상에 상태 효과(무적·침묵·속박·기절·실명 등) 부여 |
| 정글·오브젝트 | 중립 몬스터·정글 식물·유충·전령·바론·드래곤(속성별)·장로 소환, **역할 퀘스트 완료**(V26.01 추가) |
| 기타 | 아이템·룬 스택 채우기, 평타 사거리 표시, 봇 최대 9명(입문·초급·중급), 멀티플레이 최대 10명(V25.S1.1~) |
| 없음 | **위치·상황 저장/불러오기는 위키에 없다**(같은 상황 반복 연습은 수동 재현 필요) |

- 드릴 설계 함의: "막타 타이밍·스킬 콤보·사거리 감각"은 연습 모드로 반복 가능(속도 0.25배·쿨 초기화·허수아비). "웨이브 상태·적 챔피언 반응"이 필요한 드릴(프리징·딜교 창)은 봇·멀티플레이로만 근사할 수 있다. 연습 모드 과제 자동 측정(plan TR-04)은 Live Client Data API가 연습 모드에서도 동작해야 성립한다 → 동작 여부 `[확인 못 함]`(I장 질문 Q9).


---

## E. 시장

### E-1. 한국 LoL 이용 지표 (출처 있는 것만)

| 지표 | 값 | 출처 (확인일) |
|---|---|---|
| PC방 점유율(게임트릭스) | **리그 오브 레전드 1위 37.04%**, **431주 1위**. 2위 FC온라인 9.02%, 3위 배틀그라운드 8.26%, 4위 발로란트 7.78%, 5위 오버워치 5.81% (데이터 기준일 2026-11-02) | https://www.gametrics.com/ (2026-11-02) |
| PC방 사용시간 점유율(주간 기사) | LoL 37.86%, 1위 유지 (게임트릭스 주간 자료 인용 기사) `[2차]` | https://www.khgames.co.kr/news/articleView.html?idxno=309921 (2026-11-02) |
| 국민 게임 이용률(2025 게임이용자 실태조사) | 만 10~69세 10,000명 조사. 게임 이용률 **50.2%**(전년 대비 −9.7%p, 2015년 집계 이래 최저 보도), **PC 이용률 58.1%**(+4.3%p), PC 일평균 이용 시간 주중 117.9분·주말 193.4분(최근 5년 최고). 2025-12-18 발표 | https://v.daum.net/v/0C1WCBCEhA , https://biz.sbs.co.kr/amp/article/20000280068 `[2차, 보고서 원문 미확인]` |
| LoL 개인 이용자 수·KR 랭크 인원·티어 분포 | 공식 수치를 찾지 못함 | `[확인 못 함]` |
| KR 솔로랭크 상위 티어 정원 | **그랜드마스터 700 · 챌린저 300**(정원 기준 GM+ 최대 1,000명), 마스터는 정원 없음 → 마스터 인원 `[확인 못 함]` | C-8 참조, Riot 고객지원(2026-05-30 게시) |
| 참고: 표본 규모 | LoLalytics 26.21 에메랄드+ 챔피언 표본 24,575,060개(전 세계) | C-8 참조 |

> 주의: Skill Capped 사이트의 "전체 LoL 플레이어 67M, 랭크 4.5M"은 날짜·지역·근거가 없는 자사 주장이라 쓰지 않는다. https://www.skill-capped.com/lol/landing

### E-2. 코칭·학습 서비스 지불 의향 근거 (간접 근거)

설문으로 측정한 "게이머의 코칭 지불률" 통계는 **찾지 못했다** `[확인 못 함]`. 아래는 시장에서 관찰되는 간접 근거다.

| 근거 | 내용 | 출처 (확인일 2026-11-02) |
|---|---|---|
| 국내 AI 코칭 유료 등급 | YOUR.GG 월 7,900 / 9,900 / 24,900원 3단 구독, 이용자 1만 명·리포트 27만 개 `[자사 주장]` | https://your.gg/ko/kr/membership |
| 팀이 코칭 앱 인수 | **젠지 → YOUR.GG(지지틱스) 인수**(2024-04~05 보도, 아카데미 육성·스카우팅 목적, 금액 비공개) | https://gamesbeat.com/geng-acquires-league-of-legends-platform-yourgg/ , https://www.xportsnews.com/article/1853371 |
| 팀이 AI 코칭 인수 | **GIANTX → iTero Gaming 인수**(2024-07-24). iTero는 2022-09 출시 후 2년이 안 돼 약 25만 설치 | https://giantx.gg/en/blogs/news/giantx-acquires-league-of-legends-ai-coaching-start-up-itero-gaming |
| 해외 지표형 구독 | Mobalytics Plus 월 $9.99(연 $69.99). 2020년 시리즈 A 1,125만 달러 | https://mobalytics.gg/lol/glp/plus , https://www.vcaonline.com/news/2020072211/mobalytics-raises-11-25m-series-a-to-build-worlds-best-personal-gaming-assistant/ |
| 강의형 구독 | Skill Capped "월 $8.25부터" | https://www.skill-capped.com/lol/landing |
| 통계형 구독 | U.GG 월 $3.99 / 연 $29.88, Blitz 월 $3.99 → **광고 제거·편의 기능** 위주의 저가대 | https://u.gg/plus , https://blitz.gg/premium |
| 사람 코칭 단가 | Metafy LoL VOD 리뷰 약 $12~$60/세션 `[2차]`, 숨고 게임 레슨 평균 시간당 20,000원(게임 전반) `[2차]` | https://metafy.gg/@apples/sessions/replay-review-tEnneV12Mob , https://soomgo.com/a.%EC%84%9C%EC%9A%B8-%EA%B4%80%EC%95%85%EA%B5%AC/%EA%B2%8C%EC%9E%84-%EB%A0%88%EC%8A%A8 |
| 반대 근거 | **ProGuides 2024-05-29 코칭 플랫폼 종료**(Coachify로 이관) → 사람 코치 마켓 모델의 지속성 리스크 | https://www.dodge.gg/valorant/news/is-proguides-worth-it-for-valorant-2026 , https://app.dealroom.co/companies/proguides |

**해석** `[조사원 해석]`
- 가격대가 둘로 나뉜다. **"데이터 열람·광고 제거"는 월 $2.5~4**, **"개인화 진단·코칭"은 월 7,900~24,900원 / $8~10**. GRIMOIRE 유료 등급은 후자에 속해야 하고, 그 근거는 '개인 처방'이어야 한다.
- e스포츠 팀(젠지, GIANTX)이 코칭 앱을 사들이는 흐름은 **"육성·측정 도구"로서의 가치**가 인정받는다는 신호다. B2B(아카데미·아마추어 팀) 채널 가능성.
- 지불 의향을 직접 보여 주는 통계가 없으므로 **기획 단계에서 1차 설문·랜딩 페이지 테스트가 필요**하다(→ plan.md).

### E-3. 벤치마크 ① 체스닷컴 — '복기 → 처방' 모델

**규모와 성장**

| 지표 | 값 | 출처 (확인일 2026-11-02~03) |
|---|---|---|
| 회원 | 2025-04 **2억 명 돌파**, 그중 **유료 150만 명** → 유료 비율 약 0.75% `[계산]` | https://techcrunch.com/?p=2998565 (2025-04-24) |
| 회원(최신) | **2026-06-30 기준 2억 6,700만+**, Q2 평균 **DAU 1,020만**(전년 대비 +25.7%), Q2 가입 1,420만 | https://www.chess.com/board-reports/2026-q2 |
| 성장 요인 | 2021년 AI 게임 리뷰 출시. 신규 이용자 대부분이 초보이고, 이 기능이 입문자 정착을 돕는다는 회사 측 설명. 신규 가입의 약 85%는 미국 밖 | TechCrunch(위) |
| 매출 | 비상장, **공식 매출 공개 없음** `[확인 못 함]` | — |

**Game Review의 구조** (= GRIMOIRE '복기'의 참조 모델)
- v2(2023-03): Coach 요약, 그 판의 퍼포먼스 레이팅, **오프닝·미들게임·엔드게임 단계별 등급**, Coach 해설 속 단어에 마우스를 대면 보드에 화살표·칸 강조, 수 분류 재정의(예: blunder는 평가치 급락 + 기물 손실·메이트 허용이어야 함, 새 분류 "miss" = 상대 실수를 응징하지 못한 수). https://www.chess.com/news/view/chesscom-launches-game-review-v2
- 수 분류는 **기대 점수(Expected Points) 모델** 기반이고, **초보에게는 'Great' 판정을 더 관대하게** 준다. 핵심 수(Key moves)에서 더 나은 수가 있었으면 **'Retry'로 그 자리에서 다시 두게** 하고, 재시도를 반영한 보정 정확도를 보여 준다. https://support.chess.com/en/articles/8572705-how-does-game-review-work , https://www.chess.com/terms/game-review

**무료/유료 게이팅** (지원 문서 2026-07-21 기준, 현 플랜은 2022-09-01부터 시행)

| 등급 | 복기 | 레슨 | 퍼즐 | 코치 | 기타 |
|---|---|---|---|---|---|
| Basic(무료) | **하루 1회** 전체 리뷰 | 입문 섹션 + 하루 1개 | 하루 3개 | 월 1게임 | — |
| Gold | (Basic과 같음) | 무제한 | 무제한 | 무제한 | 광고 제거 |
| Platinum | **무제한 리뷰** | 무제한 | 무제한 | 무제한 | — |
| Diamond | 무제한 + **모든 수에 Coach 해설** | 무제한 | 무제한 | 무제한 | **Insights**(전체 경기 통계), 고급 통계 |

- 한국 원화 가격(연간 결제 시 월 환산, 세금 포함): **Gold ₩4,834 · Platinum ₩7,750 · Diamond ₩11,667 · 가족(2~6명) ₩20,000**, 7일 무료 체험(5일째 안내). 페이지 문구 "Premium으로 레이팅 2.7배 빠르게" `[자사 주장]`. https://www.chess.com/membership (2026-11-03, 브라우저에서 확인)
- 출처: https://support.chess.com/en/articles/8562418

**GRIMOIRE 대응표** `[조사원 제안]`

| 체스닷컴 | GRIMOIRE 대응 | 메모 |
|---|---|---|
| Game Review(하루 1회 무료) | 복기 리포트 하루 1판 무료 | Riot 무료 등급 의무와도 맞음 |
| Key moves + 분류(Brilliant~Blunder, Miss) | 결정적 장면 + 판단 등급(정석/좋음/부정확/실수/놓침) | 초보 관대 판정 규칙 차용 |
| 단계별 등급(오프닝·미들·엔드) | 라인전·운영·한타 단계 등급 | 시간대 기준은 D-3 시트 |
| Retry | 같은 상황을 판단 시뮬레이션으로 재도전 | 처방 → 수련 연결점 |
| Puzzles | 상황 판단 드릴(원자 개념별) | 하루 3개 무료 |
| Lessons(커리큘럼) | 마도서 | 하루 1레슨 무료 |
| Insights | 성장 기록(원자 지표 추세) | 상위 등급 |
| Coach 해설(Diamond) | LLM 해설 + 근거 통계 | 상위 등급 |

### E-4. 벤치마크 ② 듀오링고 — 학습 리텐션 설계

**규모·수익 구조 (FY2025 10-K, 2025-12-31 기준)** https://www.sec.gov/Archives/edgar/data/1562088/000162828026012494/duol-20251231.htm (확인일 2026-11-03)

| 지표 | 값 |
|---|---|
| 총매출 | **$1,037.6M**(2024년 $748.0M) |
| 구독 매출 | **$873.4M** → 총매출의 약 84% `[계산]` |
| 광고 매출 비중 | 약 7.7% |
| 유료 구독자 | **1,220만**(2024년 950만) |
| Q4 평균 MAU / DAU | **1억 3,310만 / 5,270만** → DAU/MAU 약 40% `[계산]` |
| 유료 비율 | MAU의 **약 9%** |
| 스트릭 | **7일 이상 스트릭 DAU 약 4,300만**, 365일 이상 약 1,500만 |
| 모델 | 모든 코스 무료(레슨 끝 광고), 유료(Super, Max)는 광고 제거 + 추가 기능 |

**스트릭 실험 결과 (듀오링고 공식 블로그)**

| 실험·관찰 | 결과 | 출처 |
|---|---|---|
| 7일 스트릭 도달 | 다음 날 사용 확률 **2.4배** | https://blog.duolingo.com/improving-the-streak/ (2020-11-19) |
| 스트릭을 일일 목표와 분리(레슨 1개면 스트릭 유지) | **D14 리텐션 +3.3%**, 스트릭 중인 일일 학습자 +10.5%(20일 내), 신규 학습자 +19%, 7일+ 스트릭 인원 40% 이상 증가 | 같은 글 |
| 스트릭 내기(Streak Wager) | 7일 유지에 재화를 걸게 하자 **D7 리텐션 +14%**(D1·D14도 유의미하게 상승) | https://blog.duolingo.com/how-streaks-keep-duolingo-learners-committed-to-their-language-goals (2017-05-10) |
| 주말 부적(Weekend Amulet) | 1주 뒤 복귀 +4%, 스트릭 유실 −5% | 같은 글 |
| 몰아서 학습 | 몰아서 하는 학습자가 꾸준히 하는 학습자보다 이탈 확률이 훨씬 높음(수치 미공개) | 같은 글 |

**GRIMOIRE 적용** `[조사원 제안, 3단계 리뷰 R-15 반영]`
- **기본은 주간 학습 리듬**(plan 6.5의 주 3일 리듬)이다. **일일 스트릭은 기본값이 아니라 V1 옵트인 실험**으로만 둔다. 일일 스트릭의 손실 공포가 plan 6.1 원칙(손실 공포 금지)과 curriculum S5.MENTAL.STOP.03(연패 후 쉬기)에 어긋나기 때문이다. 사용자 결정 D2와 연결된다.
- 듀오링고 근거는 '리듬의 최소 단위'에 흡수한다: **학습한 날 = 드릴 1개 또는 레슨 1개.** 랭크 게임을 안 한 날에도 채울 수 있어야 한다(스트릭과 일일 목표 분리 실험 D14 +3.3%).
- **휴식권 자동 지급**(주말 부적 효과: 1주 뒤 복귀 +4%, 유실 −5%). 패치 데이·시험 기간에도 쓴다.
- 복기는 "하루 1판 깊게"가 기본(몰아보기 이탈 신호). 연패 중에는 복기 대신 짧은 드릴이나 휴식을 권하는 '틸트 방지' 규칙.
- 듀오링고(유료 9%)와 체스닷컴(유료 약 0.75%)의 차이가 크다 → 전환율 가정은 보수적으로 잡고, 무료 등급의 학습 가치로 리텐션을 먼저 확보한다.

---

## F. 시사점

### F-1. 기획자(plan.md)에게
- **포지셔닝**: "AI가 내 판을 코칭"은 YOUR.GG가 2026-10에 먼저 출시했다. 메시지는 **"교과서 + 근거 + 처방 루프"**로 잡는다. "절대 질 수 없는"을 그대로 쓰지 말고 근거·신뢰도 표시로 대체한다. 이는 Riot의 "결정 지시 금지"·"선택지 제시" 원칙과도 맞는다.
- **무료 등급은 필수**(Riot 정책). 체스닷컴식으로 **하루 1판 복기 + 하루 1레슨 + 하루 3드릴 무료**, 유료는 무제한 복기·원자 진단·처방 커리큘럼·성장 리포트.
- **가격 앵커**: YOUR.GG 월 7,900/9,900/24,900원, 체스닷컴 KR 월 4,834~11,667원(연간). 통계형($2.5~4)보다 위, 사람 코칭(시간당 2만 원~)보다 아래.
- **출시 순서 근거**: (1) 웹 — match-v5 기반 매크로 복기 + 마도서(정책·기술 리스크 가장 낮음) → (2) 모바일 — 드릴·스트릭·리포트 열람 → (3) PC 컴패니언 — 리플레이 렌더 기반 미시 분석(Windows 전용, 패치 2주 창, 최근 20판, 다운로드 시간당 10회 제약).
- **MVP에서 뺄 것**: 인게임 오버레이·실시간 음성 코칭·적 타이머·적 정글 추적 알림(B-4). 증강·아레나 승률. MMR·숨은 점수 추정.
- **Riot 등록 일정**: Production 키 심사에는 작동하는 프로토타입이 필요하다 → 비공개 프로토타입(Personal 키) → 제품 등록(AI 사용 방식 명시) → 승인 후 공개 베타. 새 기능마다 재감사.
- **이름·브랜딩**: 'GRIMOIRE — 협곡의 마도서'는 정책상 문제 없어 보인다. 도메인·앱 이름·SNS 계정·검색 광고 키워드에 LoL/League/Riot을 넣지 않는다. 법적 고지는 모든 화면 하단·스토어 설명에 넣는다.
- **B2B 기회**: 젠지(YOUR.GG)·GIANTX(iTero) 인수 사례 → 아카데미·아마추어 팀용 플랜 검토.
- **해야 할 1차 조사**: 지불 의향 설문(아이언~골드), 랜딩 페이지 가격 테스트. 공개 통계가 없다.
- **법무·Riot 확인**: H장 체크리스트(만 14세 미만, 국외 이전, 다른 9명 데이터, 구독 갱신·철회, 앱 마켓 결제, AI 기본법 고지)와 I장 DevRel 질문 12개를 의존성 표에 넣는다. 출시 전 변호사 검토가 착수 조건이다.

### F-2. 교과서 설계자(curriculum.md)에게
- **2025년 이전 자료는 그대로 쓰면 틀린다.** 아타칸·힘의 위업 제거, 포탑 방패 14분 소멸 폐지, 첫 미니언 0:30·정글 0:55·바론 20:00, 정글 펫 진화 15/35, 치명타 200% 등(D-2).
- 모든 수치에 `@26.xx` 태그를 달고 D-3의 `[검증 필요]` 항목은 26.22 노트(2026-11-04 예정)로 재확인한다.
- **역할 퀘스트가 라인전의 '왜'를 바꿨다**: 라인 체류 패시브 포인트, 라인 밖 오브젝트 −75% 페널티(진행도에 따라 감소), 라인 2분당 로밍 1분 면제, 귀환 후 12초 정지. 로밍·귀환 판단 원자 개념에 **비용 변수로 명시**한다. 탑은 레벨 상한 20과 해방된 순간이동, 미드는 공격력·주문력 +8%와 3단계 신발.
- **WASD가 랭크에서 허용됐다(26.09).** '무빙·카이팅·백무빙' 레슨은 클릭 이동과 WASD 두 조작계를 모두 다룬다.
- **측정 가능성 등급**을 레슨마다 표시하기를 권한다(엔진 1:1 규칙용): (a) match-v5로 측정(분당 CS·골드 차·구매/귀환 타이밍·와드·데스 위치와 피해 구성·오브젝트 타이밍) / (b) Live Client 로컬 기록으로 보강(본인 골드·아이템·레벨의 초 단위 변화) / (c) 리플레이 CV 필요(백무빙·딜교 창·스킬 회피·어그로 핑퐁).
- 체스닷컴식 **판단 등급 체계와 초보 관대 판정**, **Retry(같은 상황 재도전)**를 드릴 설계에 차용한다.
- 프로 VOD를 레슨에 직접 넣지 말고, 데이터로 다시 그린 2D 미니맵 도식으로 설명한다(팬 콘텐츠 정책).

### F-3. AI 아키텍트(engine.md)에게
- **와드 위치는 어떤 공식 경로로도 얻을 수 없다**(C-1: `WARD_PLACED`에 위치 없음, C-2: Live Client에 좌표 없음). 시야 원자는 R2에서 '와드 설치 시각·개수·사망 직전 설치 여부'로만 잰다.
- **데이터 계층 3단**: L1 match-v5 + timeline(서버, 공식, 1분 프레임·ms 이벤트) / L2 Live Client Data API 로컬 기록(본인 중심, 좌표 없음, 게임 중 수집만) / L3 Replay API 재생 + 녹화 + CV(미시 해상도, 같은 패치 안에서만, Windows 전용).
- **쓰지 말 것**: 게임 메모리 읽기(2026-10-06 차단, 승인 앱도 2027-04 유예 종료), .rofl 직접 해독(암호화·하위 호환 없음), LCU 핵심 의존(비공식), 리플레이 대량 다운로드(시간당 10회, Riot이 "대규모 스크래핑"을 명시적으로 경계).
- **API 키**: 서버에만 보관, 배포 바이너리 금지, 제품당 키 하나, 메서드 리밋은 응답 헤더로 측정. KR → ASIA 리전 라우팅은 Reference 페이지에서 검증.
- **정석 엔진 표본**: KR 챌린저·그랜드마스터 솔로랭크(queue 420)를 주 표본으로, 에메랄드+를 비교군으로. Personal 키 상한은 하루 약 3.6만 경기 `[계산]`이므로 패치(2주) 단위 표본 설계가 필요하다. 프로 위치 데이터는 GRID 상업 계약 없이는 얻을 수 없다(LCK는 미디어 한정) → MVP 제외.
- **큐 필터**: League Classic(26.15~), 아레나, 아수라장, 사용자 설정 게임 제외. 사용자 설정 게임 전적은 RSO 본인만.
- **LLM 사용**: 정책 금지 조항은 없지만 Riot은 AI 자동화·계측을 경계한다. 외부 LLM에는 비식별 경기 피처만 보내고, 학습에 쓰지 않으며, 등록 양식에 명시하고 DevRel 확인을 받는다. 기준선은 YOUR.GG 영상 AI 코칭(장면 단위 + 클릭 가능한 타임스탬프 + 후속 질문)이고, 우리는 여기에 **근거 수치(n·Δ승률·신뢰도)**와 **원자 개념 ID**를 더해야 한다.
- **MMR처럼 보이는 단일 실력 점수 금지** → 원자 개념별 숙련도 벡터로 설계한다.
- **개인정보**: RSO 로그인, 동의 기반 수집, Riot이 전달하는 삭제 요청 처리 파이프라인, 해지 시 전량 삭제.

### F-4. 리뷰어(review.md)에게 — 검증 체크리스트
- [ ] 기획 문서에 실시간 인게임 기능(오버레이·음성)이 MVP로 들어가 있지 않은가 (B-4)
- [ ] "절대 질 수 없는", "보장" 같은 표현 대신 근거·신뢰도 표시를 하는가
- [ ] 2025 이전 수치(아타칸, 14분 방패, 1:05 미니언, 25:00 바론)가 남아 있지 않은가 (D-2)
- [ ] 미시 피드백 예시("백무빙 1회")가 L3(리플레이 CV) 경로와 그 제약(패치 창·20판·10/h)을 전제로 하는가 (C-1, C-3)
- [ ] 무료 등급, 법적 고지, 상표 미사용이 지켜지는가 (B-3, B-6)
- [ ] 프로 데이터·영상 사용 계획이 GRID 계약·팬 콘텐츠 정책과 충돌하지 않는가 (C-6, C-7)

---

## G. 확인하지 못한 점 · 출처가 엇갈린 점 (한곳에 모음)

| 항목 | 상태 |
|---|---|
| Development 키 레이트 리밋 | 포털 무기재 / 약관 "10초 10회" / 서드파티 "1초 20·2분 100" → 엇갈림 |
| match-v5 메서드별 리밋, KR의 리전 라우팅 | 공식 수치 미확인 |
| 타임라인 이벤트 타입 전체 열거값 | 일부만 확인(나머지는 필드로 추정) |
| Live Client Data API 현재 체력 필드 | **해결**(2026-11-03): `activeplayer.championStats.currentHealth`·`maxHealth` 있음, 본인만(C-2) |
| Live Client Data API의 리플레이 재생 중·연습 모드 중 동작 여부 | 미확인 → I장 Q9 |
| `WARD_PLACED`·`WARD_KILL` 위치 필드 | **해결**(2026-11-03): 위치 없음(C-1). 현 패치 실응답 1건 재확인은 엔진 담당(키 필요) |
| "data broker 금지" 원문 | 검색 요약에서만 확인 → 원문 재확인 필요 |
| 26.20 "서드파티 리플레이 접근 완전 차단" 보도 | 26.21에서 시간당 10회 제한으로 대체된 것으로 보이며, 이후 재강화 가능성은 미확인 |
| YOUR.GG 적 정글 감지 알림, STATUP.GG 실시간 코칭의 Riot 승인 여부 | 미확인 |
| OP.GG·DeepLoL·Porofessor 유료 가격, OP.GG AI 기능 | 미확인 |
| GRID 가격, NAVER×GRID 제휴의 LCK 포함 여부, LCK 영상 이용 조건 | 미확인 |
| Oracle's Elixir·Leaguepedia 2026 데이터 갱신·라이선스 | 미확인 |
| 결정질 과성장 활성 시점, 원소 드래곤 첫 등장, 일반 캠프 리스폰·골드·XP | **해결**(2026-11-03): 90초 = 게임 시각 1:40(같은 시점), 드래곤 5:00, 캠프 값은 D-3 ② |
| 억제기 재생성, 사망 타이머의 26.x 변경, 전령 소멸 19:45, 드래곤 영혼 효과 현재값 | 위키 미기재 또는 엇갈림 |
| 26.10 미니언 어그로 변경 | **해결**(2026-11-03): D-2에 추가 |
| KR 챌린저·그랜드마스터 정원 | **해결**(2026-11-03): 솔로랭크 700·300, 자유 랭크 500·200(C-8) |
| KR 마스터 인원(마스터+ 전체), KR 랭크 인구 | 미확인. 정확한 값은 `league-v4` 응답 수로만 확인 가능(키 필요, 엔진 담당) |
| 연습 모드 기능 범위 | **부분 해결**(2026-11-03): 위키 기준 목록(D-4). 상황 저장 기능 없음, Live Client 동작 여부 미확인 |
| 한국 LoL 개인 이용자 수(실태조사 원문), 코칭 지불 의향 설문 | 미확인 |
| 체스닷컴 매출 | 비공개 |
| 한국 개인정보보호법 적용 상세 | **H장 법무 체크리스트로 정리**(조사이며 법률 자문 아님). 항목별 '확인 상태'는 H장 표 |
| 전자상거래법 시행령의 유료 전환 고지 기간(14일 vs 30일), 다크패턴 과태료 액수 | 출처 엇갈림(H장 H-3) |

---

## H. 법무 체크리스트 (3단계 리뷰 R-11)

> **이 장은 공개 자료 조사 결과이지 법률 자문이 아니다.** 조문 해석·적용 여부는 **출시 전 변호사 검토가 전제**다(plan 7.4 의존성 표의 '착수 조건'으로 넣는 것은 기획자 몫). 조문은 국가법령정보센터 원문으로 다시 확인할 것.
> 확인 상태: **●** 조사원이 조문 원문 또는 운영사 공식 문서로 확인 · **◐** 언론·로펌·법률 해설 등 2차 출처로만 확인 · **○** 조문을 열람하지 못함(번호·내용 재확인 필요) 또는 해석이 필요함. 확인일은 모두 2026-11-03.
> 담당 단계는 plan.md 기능 ID와 로드맵(MVP/V1/V2/V3) 기준이다.

### H-1. 개인정보 (개인정보 보호법)

| # | 항목 | 근거 법령·내용 | 담당 단계 (기능) | 확인 상태 | GRIMOIRE 메모 |
|---|---|---|---|---|---|
| L1 | 수집·이용 근거와 동의 항목 | 제15조 제1항: 동의(1호), 계약 체결·이행(4호), 정당한 이익(6호) 등 7개 근거 | MVP (CM-01 RSO, RC-01 수집) | ● 조문 확인 | 서비스 필수 항목(PUUID·Riot ID·경기 기록)은 계약 이행, 선택 항목(마케팅, AI 질문 기록 보관, 컴패니언 로컬 기록 업로드)은 별도 동의로 나누는 안 → 변호사 판단 |
| L2 | **만 14세 미만** | 제22조의2: 동의로 처리할 때 **법정대리인 동의를 받고 확인**해야 함. 동의에 필요한 최소 정보는 아동에게 직접 수집 가능. 아동에게는 이해하기 쉬운 표현으로 고지 | MVP (가입 흐름) | ● 조문 확인 | 선택지: 만 14세 미만 가입 제한 vs 법정대리인 동의 흐름. Riot 계정 자체의 연령 정책은 `[확인 못 함]` |
| L3 | **국외 이전** (미국 LLM API·해외 클라우드) | 제28조의8(2023-09-15 시행): 원칙 금지, 예외 중 하나가 "계약 체결·이행을 위한 처리위탁·보관" → **처리방침 공개 또는 전자우편 등으로 고지**. 고지 항목: 이전 항목, 이전 국가·시기·방법, 받는 자 명칭·연락처, 이용 목적·보유 기간, **이전 거부 방법·절차·효과**. 그 밖에 별도 동의, 인증, 동등성 인정 경로 | MVP (RC-02·RC-03 LLM 문장 생성, 서버 호스팅) | ● 조문 확인 | LLM에 보내는 데이터가 '비식별 피처'라도 개인정보에 해당하는지가 첫 질문. 해당하면 처리방침에 제공사·국가 명시 |
| L4 | 처리 위탁 고지 | 업무위탁 조항(제26조로 알려짐): 위탁 업무·수탁자 공개 | MVP (LLM 제공사, 클라우드, 메일 발송, 결제대행) | ○ 조문 미열람 | 수탁자 목록을 처리방침에 표로 유지 |
| L5 | **같은 게임 다른 9명의 데이터** | 동의를 받을 수 없으므로 **제15조 제1항 제6호(정당한 이익)**가 후보: 정당한 이익을 위해 필요하고 **명백하게 정보주체의 권리보다 우선**하며, 이익과 **상당한 관련이 있고 합리적 범위**를 넘지 않을 것. 개인정보위 'AI 개발·서비스를 위한 공개된 개인정보 처리 안내서'(2024-07-17)가 공개 데이터 활용의 근거로 이 조항을 제시했다는 보도. 통계·연구 목적의 가명정보 처리 조항(제28조의2로 알려짐)도 후보 | MVP (RC-02 진단에 상대 정보), V1 (CN-01 정석 집계) | ◐ 조문 6호는 ●, 안내서는 보도로만 확인 | Riot ID가 다른 정보와 결합돼 식별 가능하면 개인정보. 최소화 원칙: 타인은 챔피언·역할·집계 수치로만 표시, **타인 프로필화 금지**(plan K12), 화면의 타인 Riot ID 노출 최소화. Riot 정책(숨긴 플레이어 분석 금지, B-4)과 함께 지킨다 |
| L6 | 상위권 대량 수집 (C-8) | L5와 같은 근거 + 가명처리 후 집계 | V1 (CN-01~05) | ◐ | 원자료 보존 기간을 패치 단위로 제한, 집계 결과만 장기 보관하는 안 |
| L7 | 자동화된 결정 | 제37조의2(2024-03-15 시행): AI 포함 **완전히 자동화된 결정**이 권리·의무에 **중대한 영향**을 미치면 거부·설명 요구 가능 | MVP (RC-02 진단, TR-01 처방) | ● 조문 확인 · ○ 해당 여부 | 학습 진단·처방은 '중대한 영향'에 해당하지 않을 가능성이 높다는 것은 조사원 해석. 이의 제기(RC-09)·근거 패널(CN-02)은 설명 요구 대응 장치로도 쓸 수 있다 |
| L8 | 처리방침 공개·보호책임자 지정 | 처리방침(제30조로 알려짐), 보호책임자(제31조로 알려짐) | MVP 출시 전 | ○ 조문 미열람 | L1~L6 내용을 처리방침에 반영 |
| L9 | 파기·열람·정정·삭제 요청 | 파기·열람·삭제 조항(제21조·제35조~제37조로 알려짐) + **Riot API 약관의 삭제 요청 전달 의무**(B-8) | MVP (CM-06 데이터·개인정보 센터) | ○ 조문 미열람 · Riot 쪽 ● | 탈퇴 시 전량 삭제, Riot이 전달한 삭제 요청 처리 파이프라인 |
| L10 | 안전성 확보 조치 | 안전조치 조항(제29조로 알려짐) | MVP | ○ 조문 미열람 | 키·토큰은 서버에만(B-2와 같은 원칙) |

출처: 제15조 https://casenote.kr/법령/개인정보_보호법/제15조 · 제22조의2 https://casenote.kr/법령/개인정보_보호법/제22조의2 · 제28조의8 https://casenote.kr/법령/개인정보_보호법/제28조의8 · 제37조의2 https://casenote.kr/법령/개인정보_보호법/제37조의2 (casenote 페이지에 "개정 2026. 9. 8." 표시가 있어 최신 개정 반영 여부를 원문으로 재확인 필요) · 공개된 개인정보 안내서 보도 https://newsseoul.co.kr/news/view/1065606019743236 , https://shinkim.com/kor/media/newsletter/2517 `[2차]`

### H-2. AI 서비스 (인공지능 기본법)

| # | 항목 | 근거·내용 | 담당 단계 | 확인 상태 | 메모 |
|---|---|---|---|---|---|
| L11 | 생성형 AI **사전 고지·결과물 표시** | 인공지능 기본법 **2026-01-22 시행**. 투명성 확보 의무(제31조로 보도): 생성형 AI로 운용된다는 사실을 **약관·계약서 명시 또는 앱 화면 안내**로 사전 고지. 서비스 안에서만 쓰는 생성물은 UI·로고 등으로 표시 가능, 외부로 반출되는 생성물은 워터마크·메타데이터 표시. 대상은 이용자에게 AI 제품·서비스를 직접 제공하는 '인공지능사업자'(국내 이용자 대상 해외 사업자 포함). **최소 1년 계도기간** 동안 사실조사·과태료 유예. 과기정통부 '인공지능 투명성 확보 안내 지침' 공개(2026-01-21 보도) | MVP (RC-02·RC-03 LLM 문장, V2 RC-10 질문 답변) | ◐ 보도로만 확인. 시행령 입법예고 단계에서 조문 번호 표기가 달랐다는 해설이 있어 공포본 확인 필요 | LLM이 만든 진단 문장에 'AI 생성' 표시를 하는 것은 근거 배지(CM-10)와 함께 설계 가능. 카드 이미지를 공유(외부 반출)하면 표시 의무가 달라질 수 있음 |

출처: https://newsseoul.co.kr/news/view/1065609609648210 (2026-01-21) · https://www.skax.co.kr/insight/trend/3666 · https://www.daeryunlaw.com/newsletter/news/246 `[2차]`

### H-3. 결제·구독 (전자상거래법 등)

| # | 항목 | 근거·내용 | 담당 단계 | 확인 상태 | 메모 |
|---|---|---|---|---|---|
| L12 | 통신판매업 신고 | 전자상거래법의 통신판매업 신고 조항(제12조로 알려짐) | V1 (CM-05 구독·결제) 전 | ○ 조문 미열람 | — |
| L13 | **청약 철회** | 원칙 7일. **디지털콘텐츠는 제공이 시작되고, 사업자가 철회 불가 사실을 표시하고 시험 사용 상품 등을 제공한 경우에만** 철회 제한(제17조 제2항 제5호·제6항 단서로 해설). 구독은 미제공 기간 환급이 원칙이라는 해설, 가분적 용역의 미제공 부분 철회 가능 판례 요약 | V1 (CM-05) | ◐ 법률 칼럼·판례 요약 | 무료 등급·체험이 '시험 사용 상품'으로 인정되는지 → 변호사. 철회·해지 화면을 결제 화면만큼 쉽게 |
| L14 | **정기결제 '숨은 갱신' 금지** | 2025-02-14 시행 개정: 정기결제 **대금 증액 또는 무료→유료 전환 전**에 소비자 동의를 받고 해지 방법을 고지(법 제13조 제6항·시행령 제20조의2). 기간: 최종 시행 직전 기사는 **전환·증액 전 30일 이내**, 입법예고안 해설은 **유료 전환 14일** → 출처 엇갈림 | V1 (7일 무료 체험을 넣을 경우) | ◐ 출처 엇갈림 | 체험 → 유료 전환 시 별도 동의 화면 필수로 설계 |
| L15 | **다크패턴 금지** | 같은 개정: 반복 간섭(법 제21조의2 제1항 제5호·시행령 제27조의2), 순차 공개 가격(제21조의2 제1항 제1호·시행규칙 제11조의4) 등 6개 유형. 제재 액수는 출처마다 다름 | V1 | ◐ | 업셀 팝업 반복, 해지 버튼 숨기기 금지 → design.md 영향 |
| L16 | 이용약관 | 약관규제법 + 전자상거래법상 소비자에게 불리한 약관 무효 조항(제35조로 해설) | MVP 이용약관, V3 코치 마켓 약관 | ○ | — |
| L17 | 코치 마켓 (V3) | 통신판매중개자 의무, 코치 대금을 보관·정산하면 전자금융 관련 규제 해당 여부 | V3 (CM-08) | ○ 미조사 | V3 착수 전 별도 법무 검토 |

출처: https://www.lawtimes.co.kr/news/articleView.html?idxno=205545 (2025-02-13) · https://shinkim.com/kor/media/newsletter/pdf/2583 · https://albup.co.kr/page/column_detail.php?content_id=1802 · https://korea.legal/wiki/2018%eb%8b%a4214746/ `[2차]`

### H-4. 앱 마켓 결제 규정

| # | 항목 | 내용 | 담당 단계 | 확인 상태 | 메모 |
|---|---|---|---|---|---|
| L18 | Apple (한국) | **한국 스토어 전용 앱**은 StoreKit External Purchase Entitlement로 승인 결제대행사(KCP·이니시스·토스·NICE 등) 결제 가능. **수수료 26%**(부가세 제외), 매월 판매 보고, **같은 앱에서 Apple 인앱 결제와 병행 불가**, Apple을 거치지 않는다는 안내 시트 필수, 환불·구독 관리 지원은 개발자 책임. 디지털 콘텐츠의 기본 규칙(App Store 심사 지침의 인앱 결제 조항)은 이번에 열람하지 않음 | V1 (모바일 앱 결제) | ● Apple 공식 · ○ 심사 지침 | 글로벌 출시 계획이 있으면 한국 전용 옵션은 맞지 않을 수 있음 |
| L19 | Google Play (한국) | 2023-08-02부터 한국 사용자에게 대체 결제를 제공하려면 **대체 결제 API** 사용. Google Play 결제와 나란히 제공 가능, 대체 결제 시 **수수료 4%p 감면**, PCI DSS 준수·부정 거래 신고 체계 필요, 승인 거래 24시간 내 보고 | V1 | ● Google 공식 | — |
| L20 | 규제 동향 | 2021 개정 전기통신사업법(특정 결제방식 강제 금지). **2026-10-28 방송미디어통신위원회가 구글 429억 원·애플 208억 1천만 원 과징금**, 1개월 내 이행계획·3개월 내 개선 시정명령 → 외부 결제 조건이 바뀔 수 있음 | V1 결제 설계 시 재확인 | ◐ 보도 | 웹 결제(앱 밖 구독)를 기본으로 두고 앱은 열람·드릴 중심으로 하는 안을 검토. 앱 안에서 웹 결제로 안내하는 링크 규정은 스토어별로 `[확인 못 함]` |

출처: https://developer.apple.com/support/storekit-external-entitlement-kr/ · https://support.google.com/googleplay/android-developer/answer/11222040 · https://www.koreancenter.or.kr/news/articleView.html?idxno=1431144 (2026-10-28)

### H-5. 알림·마케팅, 지식재산

| # | 항목 | 내용 | 담당 단계 | 확인 상태 | 메모 |
|---|---|---|---|---|---|
| L21 | 앱 푸시 광고 | 정보통신망법 제50조: 광고성 정보는 **사전 수신 동의**(서비스 알림 동의와 분리), **21시~08시 별도 동의**, '(광고)' 표기, 수신 거부 방해 금지, 수신 동의 2년마다 확인(ISMS-P 기준 해설) | V1 (CM-04 알림, CM-03 주간 리포트 메일) | ◐ 해설 자료 | '분석 완료'·'복습' 알림(서비스)과 프로모션 알림을 처음부터 분리 |
| L22 | Riot 상표·법적 고지 | B-6 (개발자 정책·팬 콘텐츠 정책) | MVP | ● | 리뷰어가 프로토타입 고지 문구가 원문과 글자 단위로 일치함을 확인(review.md 1.4) |
| L23 | 제품명 상표 | 'GRIMOIRE'·'협곡의 마도서' 상표 출원·등록 조사(KIPRIS)는 하지 않음 | 출시 전 | ○ 미조사 | review.md R-34(기획자) |
| L24 | 폰트 라이선스 | Hahmlet·IBM Plex Sans KR·IBM Plex Mono 모두 OFL(리뷰어 확인) | MVP | ● (review.md 1.4) | 앱에 라이선스 고지 |

출처: https://korea.legal/wiki/%ec%a0%95%eb%b3%b4%ed%86%b5%ec%8b%a0%eb%a7%9d-%ec%9d%b4%ec%9a%a9%ec%b4%89%ec%a7%84-%eb%b0%8f-%ec%a0%95%eb%b3%b4%eb%b3%b4%ed%98%b8-%eb%93%b1%ec%97%90-%ea%b4%80%ed%95%9c-%eb%b2%95-21/ · https://itwiki.kr/w/ISMS-P_%EC%9D%B8%EC%A6%9D_%EA%B8%B0%EC%A4%80_3.1.7.%ED%99%8D%EB%B3%B4_%EB%B0%8F_%EB%A7%88%EC%BC%80%ED%8C%85_%EB%AA%A9%EC%A0%81_%ED%99%9C%EC%9A%A9_%EC%8B%9C_%EC%A1%B0%EC%B9%98 `[2차]`

**변호사에게 먼저 물을 것(우선순위)**: ① L5 다른 9명 데이터의 처리 근거와 화면 표시 범위 ② L3 LLM 전송 데이터가 개인정보인지, 국외 이전 고지로 충분한지 ③ L2 만 14세 미만 정책(제한 vs 법정대리인 동의) ④ L13·L14 무료 체험·구독 해지·환불 설계 ⑤ L11 AI 기본법상 GRIMOIRE가 고지 의무 대상인지.

---

## I. Riot DevRel 질문 목록 (3단계 리뷰 R-26)

> 흩어져 있던 질문(research B-4·B-7, engine 부록 A·6.2·10.3, plan 4.1·7.3·K1·K2)을 한 표로 모았다. 질문은 제품 등록 양식·포털 메시지로 보낸다. 답변은 이 표의 '회신' 칸에 날짜와 함께 기록한다.
> 순서 전제: **작동 프로토타입(Personal 키) → 제품 등록·Production 키 심사 → 외부 베타**(B-1·B-2·C-8). 이 순서를 MVP 크리티컬 패스에 넣는 것은 기획자 몫(R-26).

| Q | 질문 | 관련 기능 ID | 왜 묻나 (근거) | 막히면 대안 | 필요 시점 | 회신 |
|---|---|---|---|---|---|---|
| Q1 | **초대제 비공개 베타**(외부 사용자 수십~수백 명)를 Personal 키로 운영해도 되나? Production 심사에 필요한 '작동 프로토타입'의 기준은? | RC-01, CM-01 (MVP 전체) | Personal 키는 공개 알파·베타 불가, Production 심사에는 프로토타입이 대체로 필요(B-1) | 팀 내부·컨시어지 대상만 Personal 키로 검증하고, 외부 베타는 Production 승인 뒤로 미룸 | Phase 0 | — |
| Q2 | RSO(Riot Sign On) 사용 승인 절차·기간은? 승인 전 본인 확인을 **프로필 아이콘 변경 방식**으로 해도 되나? | CM-01, plan K1 | 사용자 설정 게임·본인 인증은 RSO 기준(B-8) | Riot ID 입력 + 아이콘 변경 확인, 본인 경기만 분석 | Phase 0 | — |
| Q3 | 경기 데이터에서 만든 **비식별 피처**(챔피언·시각·수치, Riot ID 제외)를 **외부 LLM API**로 보내 해설을 만들고, 제공사가 학습에 쓰지 않는 조건이면 허용되나? 'data broker' 금지 취지 문구와 충돌하나? 등록 양식에 어떻게 적나? | RC-02, RC-03, RC-10 | 정책에 AI 조항은 없지만 Riot은 AI 자동화를 경계(B-7). broker 문구는 원문 미확인 | LLM 없이 템플릿 문장 생성 / 자체 호스팅 모델 | MVP 등록 시 | — |
| Q4 | 정석 카드처럼 **상위권 행동 분포·승률 차이**를 보여 주는 것이 '결정 다양성 감소'·'결정 지시'에 걸리지 않으려면 어떤 형식이어야 하나? 승률 Δ 표시는 허용되나(증강·아레나 승률 금지 사례와의 차이)? | CN-01, CN-02, RC-07 | 복수 선택지 제시는 허용, 결정 지시는 금지(B-4) | 승률 숫자 대신 분포·신뢰 등급만 표시, 항상 2개 이상 선택지 | MVP | — |
| Q5 | 원자별 숙련도·성장 그래프가 **MMR·ELO 계산기 금지**에 걸리지 않나? 랭크와 나란히 보여 줘도 되나? | CM-02, CM-07 | MMR 계산기 금지(B-4) | 단일 점수 없이 원자별 벡터만, 랭크 축 제거 | MVP / V1 | — |
| Q6 | 챔피언 선택 중 **게임 클라이언트 밖 컴패니언 창**에 본인 학습 목표(정적, 상대 정보 없음)를 표시하는 것이 '실시간 분석'이나 '결정 지시'에 해당하나? | RC-08 (PC 컴패니언) | 실시간 분석 금지(B-4), plan 4.1이 확인 전까지 클라이언트 밖으로 제한 | 대기열 진입 전·게임 후에만 표시, 모바일 알림으로 대체 | V1 α | — |
| Q7 | 컴패니언이 **LCU 엔드포인트로 본인 최근 경기 리플레이 다운로드**를 트리거하는 것을 등록하면 허용되나? 등록 시 무엇을 적나? | RC-01(컴패니언), RC-06 | LCU는 비지원이지만 사용 시 등록 요구(C-4), 다운로드 시간당 10회(C-3) | 사용자가 클라이언트에서 직접 내려받게 안내하고 리플레이 폴더만 감시 | V1 α | — |
| Q8 | 사용자 PC에서 **본인 리플레이를 Replay API로 자동 재생·녹화**(앱이 `EnableReplayApi=1`을 사용자 동의 하에 켜기, 최소화·배속 재생 포함)하고 화면을 CV로 분석해도 되나? | RC-06, RC-05(V2), RC-03(V2) | Replay API는 공식이지만 자동 대량 처리에 대한 입장은 없음, Riot은 대규모 스크래핑 경계(B-5) | 사용자가 직접 켜고 수동 실행 / 불허 시 R2 고도화 + 사람 코치 컨시어지(plan K2) | **V2 착수 전 게이트** | — |
| Q9 | Live Client Data API가 **연습 모드·리플레이 재생 중**에도 동작하나? 연습 모드 과제 자동 측정에 써도 되나? | TR-04, RC-06 | 동작 여부 미확인(C-2·C-3·D-4) | 게임 후 자기 보고 + 일반 게임 지표로 대체 | V2 | — |
| Q10 | **상위권 기여 프로그램**: 동의한 마스터+ 기여자가 컴패니언으로 **자기 리플레이**(시간당 10회 안)를 로컬 처리해 **파생 상태만**(영상·원본 없음) 올리고, 우리가 집계 정석으로 써도 되나? 기여 보상(프리미엄 무상 제공)이 문제 되나? | CN-01, CN-03, CN-05 (미시 정석), engine 6.2 C-micro | 정책에 해당 조항 없음. KR GM+ 정원은 1,000명(C-8)이라 규모 가정도 확인 필요 | ① 공식 데이터 요청 경로 문의(리뷰어 제안) ② 골드셋(engine 9.1)을 미시 근거로 재사용하고 '표본 작음' 배지 ③ 편집자 키프레임 + '코치 합의' 배지 | V2 착수 전 게이트 | — |
| Q11 | 유료 등급 출시 전 **Approved/Acknowledged** 상태를 받는 절차는? 계획한 무료 등급 범위가 '무료 등급 의무'를 충족하나? | CM-05 | 수익화는 해당 상태에서만, 무료 등급 의무(B-3) | 무료로 먼저 운영하고 승인 후 과금 | V1 | — |
| Q12 | 기능을 추가할 때 **재감사 범위**(새 데이터 경로마다? 엔진 개선마다?)와 소요 기간은? | 전체 | 새 기능·변경도 재감사(B-2) | 분기 단위로 묶어 신청 | 상시 | — |

> 프로 경기 데이터·영상(CN-06)은 DevRel이 아니라 GRID·LCK 권리자 문의 대상이다(C-6·C-7). V3 전까지 보류.

---

## 변경 이력 — 3단계 리뷰 반영

| R-xx | 처리 | 위치 |
|---|---|---|
| R-18 | 26.10 미니언 어그로 변경 행 추가(공식 노트 + 위키 V26.10, 배포 2026-05-13) | D-2 |
| R-17 | 원소 드래곤 첫 등장 5:00·리스폰 5분·장로 6분 확정(위키 Dragon pit) | D-3 ①, ⑦, G |
| R-17 | 레드·칼날부리·늑대·크룩 리스폰·골드·XP 채움(curriculum 공유 노트 위키 값, 칼날부리 조사원 재확인) | D-3 ②, ⑦, G |
| R-17 | 결정질 과성장 '90초 = 게임 시각 1:40' 같은 시점으로 확정, 활성 지연 조건 추가 | D-3 ④, ⑦, G |
| R-17 | Live Client `currentHealth`·`maxHealth` 있음(본인만) | C-2, G |
| R-11 | 법무 체크리스트 신설(개인정보·AI 기본법·전자상거래·앱 마켓·알림·IP 24항목, 근거 법령·담당 단계·확인 상태). 조사이며 변호사 검토 전제 명시 | H장, B-8 국내법 행, G |
| R-26 | Riot DevRel 질문 목록 12개(질문·관련 기능 ID·근거·대안·시점·회신 칸) | I장, B-4 말미 |
| R-07 | KR 솔로랭크 GM 700·챌린저 300(자유 랭크 500·200), 마스터 정원 없음 → 마스터 인원은 확인 못 함(API 키 미사용). 기여자 규모 가정과 비교 계산 | C-8, E-1, G |
| R-15 | E-4 적용안을 "기본 주간 리듬, 일일 스트릭은 옵트인 실험, 최소 단위 드릴 1개 또는 레슨 1개, 휴식권 자동 지급"으로 수정 | E-4, A-3 W6 |
| R-02 | `WARD_PLACED`·`WARD_KILL` 위치 필드 없음 확정, 해상도 한계 문단 수정 | C-1, G |
| R-35 | 연습 모드 기능 범위(위키 기준) 신설, 상황 저장 기능 없음, Live Client 동작 미확인 | D-4, G |
| R-19·R-20 (참고) | 미니언→챔피언 피해 55%, 미니언 기본 XP, 레벨 XP 요구량, 포탑 사거리·대상 전환·포탑 아래 막타 패턴 추가(교과서 수정은 curriculum 몫) | D-3 ③·④ |
