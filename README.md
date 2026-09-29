# Developer-side-lab

Salesforce 컨설턴트로 일하면서, 여기저기 흩어져 있던 제 데이터를 한곳에 모으려고 만든 개인 AI 도구 다섯 개의 목록입니다.

> **English** — A hub for five personal AI tools I built between July and September 2026 while working as a Salesforce consultant in Seoul.
> I thought that doing agentic AI first required gathering the data about me, scattered across many places, into one place. Two tools gather outside information (Salesforce release changes, a twice-weekly discourse scan), one turns collected posts into my own judgment records, one makes all of those notes searchable again, and one gathers my own chat history on my Mac for search and stats.
> Everything runs on one Mac and keeps personal data local; the four tools other than NewsCard call models only through a subscription Claude Code CLI or a local model, with no pay-per-use API keys. Four of the five are README-only write-ups; the code stays private because it is bound to my personal files.

동작 환경: macOS 한 대 · launchd 무인 실행 · 구독형 Claude Code CLI

## 왜 만들었나

Agentic AI를 하려면 먼저 저에 대해 산재되어 있는 데이터를 한곳으로 모아야 한다고 생각했습니다.

다섯 도구는 그 "모으기"의 서로 다른 갈래입니다. newscard와 trend-scan은 바깥 정보를 모읍니다. 하나는 Salesforce 릴리스 변화를, 하나는 제 관심사에 닿는 글을 모읍니다. personal-dashboard는 모은 글을 읽고 나눈 대화에서 제 판단만 골라 판단 기록에 붙입니다. docsearch-mcp는 그렇게 쌓인 제 문서를 AI가 다시 찾을 수 있게 합니다. kakao-analytics는 가장 큰 갈래인 제 대화 기록을 제 맥 안에서 모읍니다.

공통 제약은 아래와 같습니다. 첫 줄은 newscard를 뺀 네 도구에 해당합니다.

- 종량제 API 키를 쓰지 않습니다. 모델이 필요한 자리는 이미 결제 중인 구독 CLI나 로컬 모델로 돌립니다.
- 무인 실행(launchd)은 결과를 코드로 판정할 수 있는 일만 맡습니다. 수집·추출·통계·알림입니다. 무엇을 남길지 고르는 일과 판단 기록 수정은 세션에서 제가 합니다.
- 개인 데이터는 로컬 맥에만 둡니다.
- 따로 호스팅하는 서버가 없습니다. 전부 맥 한 대에서 돌고, 대시보드는 폰에서 사설망으로만 엽니다.

## 원래 하던 방식 ↔ 만든 것

| 원래 하던 방식 | 만든 것 | 되는 정도 |
|---|---|---|
| 릴리스 노트를 읽고 우리 org의 어느 Setup 화면을 봐야 하는지 사람이 찾았다 | [salesforce-newscard](https://github.com/WJY-ador/salesforce-newscard) | 기준 org에서 그 화면을 찾아 빨간 테두리로 캡처합니다. 조치할 Setup 화면 캡처가 없으면 카드를 보내지 않습니다 |
| 제가 리포스트한 글과 지정한 사람의 글만 봤다 | [trend-scan](https://github.com/WJY-ador/trend-scan) | 화·금 자동 스캔, 17회차 누적. 최근 회차(09-29)는 후보 90건 중 5건 통과였고, 열린 질문 리서치 레인은 후보 25건이 전부 이전 회차에 나온 글이라 통과 0건이었습니다 |
| 수집한 글을 세션 채팅에서 읽고 AI 초안을 고쳤다 | [personal-dashboard](https://github.com/WJY-ador/personal-dashboard) | 한 화면에서 고르기·대화·제출. 근거가 제 말이나 제 기록이 아닌 문장은 서버가 반려합니다 |
| `grep`·`rg`로 정확한 문자열만 찾았다 | [docsearch-mcp](https://github.com/WJY-ador/docsearch-mcp) | 표현이 달라도 찾습니다. 평가 16문항 top3 기준 홀드아웃 6/8. 후보 제시용이고 정답 판정용이 아닙니다 |
| 제 대화 기록은 카톡 앱 화면 안에서만 볼 수 있었다 | [kakao-analytics](https://github.com/WJY-ador/kakao-analytics) | 앱이 제 맥에 저장해 둔 기록을 그 기기 안에서 읽어 모으고, 통계·검색을 무인으로 돕니다. 답장을 돕는 에이전트는 아직 방향일 뿐입니다 |

## 프로젝트

### 바깥 정보를 모으기 · Inbound

**[salesforce-newscard](https://github.com/WJY-ador/salesforce-newscard)** <sub>· 2026-07 시작 · 진행 중</sub><br>
Salesforce 릴리스 노트·릴리스 업데이트를 모아, 기준 org에서 확인할 Setup 화면을 찾아 빨간 테두리로 캡처하고, 카드뉴스로 Slack·Instagram에 보냅니다.<br>
<sub>Salesforce release changes → the matching Setup screen captured with a red box → card news to Slack and Instagram.</sub>

**[trend-scan](https://github.com/WJY-ador/trend-scan)** <sub>· 2026-07 시작 · 운영 중 (2026-07-22 ~ 08-11 HOLD 후 재개)</sub><br>
제가 기록해 둔 열린 질문과 관심 축으로 검색 쿼리를 만들고, 점수·신선도·중복 판정은 Python이, 통과 판정은 모델이 해서 주간 다이제스트를 남깁니다.<br>
<sub>A twice-weekly scan that compiles my open questions and interest axes into queries; Python owns scoring and freshness, the model only judges the shortlist.</sub>

### 모은 것에서 내 판단 남기기 · Judgment

**[personal-dashboard](https://github.com/WJY-ador/personal-dashboard)** <sub>· 2026-08 시작 · 운영 중</sub><br>
무인 수집기가 모은 글을 한 화면에서 고르고 대화하고, 대화에서 나온 제 판단만 판단 기록에 붙입니다. 서버는 모델을 부르지 않습니다.<br>
<sub>A single-screen board: collect → triage → conversation → judgment record. The server never calls a model and rejects sentences not grounded in my own words.</sub>

<a href="https://github.com/WJY-ador/personal-dashboard"><img src="https://github.com/WJY-ador/personal-dashboard/raw/main/screenshots/walkthrough.gif" alt="개인 대시보드 워크스루 — 끌어 놓기, 요점, 대화, 대화 끝, 정리·제출, 구조 탭 (더미 데이터)" width="600"></a><br>
<sub>더미 데이터로 띄운 사본입니다. 원본은 personal-dashboard 저장소에 있습니다.</sub>

### 내 기록을 모으고 다시 찾기 · Local data

**[docsearch-mcp](https://github.com/WJY-ador/docsearch-mcp)** <sub>· 2026-08-11 시작 · 운영 중</sub><br>
제 마크다운 문서 약 680파일을 헤딩 단위로 잘라 BM25와 로컬 임베딩으로 색인하고, Claude Code가 부르는 MCP 툴 하나로 노출합니다.<br>
<sub>Hybrid BM25 + local bge-m3 search over my markdown notes, exposed as a single MCP tool. No paid model calls.</sub>

**[kakao-analytics](https://github.com/WJY-ador/kakao-analytics)** <sub>· 2026-08-26 시작 · 운영 중</sub><br>
맥용 카카오톡이 이미 제 기기에 저장해 둔 제 대화 기록을 그 기기 안에서만 읽어 한곳에 모으고, 통계·검색을 만듭니다. 기록과 결과는 기기 밖으로 나가지 않습니다.<br>
<sub>My own chat history, gathered locally on my Mac for search and stats. Nothing leaves the machine.</sub>

## 공통으로 지키는 것

**모델을 부르는 자리를 한정합니다.** 다섯 도구 중 모델을 부르는 곳은 newscard의 이미지 생성, trend-scan의 수집·판정 두 번, dashboard의 워커, kakao-analytics의 화이트리스트 방 요약입니다. docsearch는 로컬 임베딩 모델만 씁니다. 코드가 정한 값을 모델이 바꾸지 못하는 자리를 각 README에 적었습니다.

**"보냈다"가 아니라 "남은 것"을 셉니다.** 각 README의 결과 절에는 실행 로그가 아니라 실제로 쌓인 건수와 날짜를 적었습니다. 결과가 0건이었던 회차, 껐다가 다시 켠 기간, 은퇴시킨 수집 경로도 그대로 둡니다.

**되는 정도에는 확인하지 못한 것을 같이 씁니다.** 평가셋이 작아 차이를 유의하다고 말할 수 없는 경우, 통과한 글을 제가 실제로 쓴 비율이 아직 0인 경우처럼, 한계는 그 도구 README의 「되는 정도·한계」 절에 있습니다.

<details>
<summary>타임라인</summary>

| 날짜 | 한 일 |
|---|---|
| 2026-07-04 | trend-scan 첫 다이제스트 |
| 2026-07 | salesforce-newscard 시작 |
| 2026-07-12 | trend-scan을 플랫폼별 소스 레인으로 분리 |
| 2026-07-22 | trend-scan HOLD — 토큰 대비 읽을 글이 거의 남지 않음 |
| 2026-08-11 | trend-scan 재개. 같은 날 docsearch 첫 색인·평가 |
| 2026-08 | personal-dashboard 시작 |
| 2026-08-25 | trend-scan 키워드 레인을 열린 질문 리서치 레인으로 교체 |
| 2026-08-26 | kakao-analytics 첫 판 — 로컬 기록 모으기·통계·무인 실행 |
| 2026-09-29 | trend-scan 취향 쿼리 추가, 17회차 |

</details>

## 이 허브에 없는 것

- 코드. newscard만 코드가 공개돼 있고, 나머지 네 개는 README만 있습니다. 개인 기록 파일 형식에 묶여 떼어 내면 돌지 않습니다.
- 수집한 원문, 제 판단 기록 본문, 대화 기록.
- 고객사 org와 고객 데이터. 캡처는 데모·테스트 org와 더미 데이터로만 찍었습니다.
</content>
</invoke>
