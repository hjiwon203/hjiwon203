<div align="center">

# 황지원(Hwang Ji-won) | Backend Developer

**이유 있는 설계를 하는 백엔드 개발자입니다.**<br/>
메시지 파이프라인의 실패 격리부터 협업 서비스의 도메인 설계, 비동기 생성 파이프라인까지 "왜 이렇게 만들었는지"를 설명할 수 있게 만들어 왔습니다.

<!-- 이메일·블로그가 있으면 아래 주석을 풀고 주소를 바꿔 주세요 -->
<!--
<a href="mailto:you@example.com"><img src="https://img.shields.io/badge/Email-EA4335?style=flat-square&logo=gmail&logoColor=white"/></a>
<a href="https://your-blog.example"><img src="https://img.shields.io/badge/Blog-000000?style=flat-square&logo=velog&logoColor=white"/></a>
-->

</div>

---

## 🛠 Tech Stack

| 분류 | 기술 |
|---|---|
| **Language** | ![Java](https://img.shields.io/badge/Java_21-007396?style=flat-square&logo=openjdk&logoColor=white) |
| **Framework** | ![Spring Boot](https://img.shields.io/badge/Spring_Boot_3-6DB33F?style=flat-square&logo=springboot&logoColor=white) ![Spring Data JPA](https://img.shields.io/badge/Spring_Data_JPA-6DB33F?style=flat-square&logo=spring&logoColor=white) ![Spring Cloud](https://img.shields.io/badge/Spring_Cloud-6DB33F?style=flat-square&logo=spring&logoColor=white) |
| **Messaging / Cache** | ![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white) ![Firebase Cloud Messaging](https://img.shields.io/badge/Firebase_FCM-FFCA28?style=flat-square&logo=firebase&logoColor=black) |
| **Database** | ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white) ![PostgreSQL](https://img.shields.io/badge/PostgreSQL_+_pgvector-4169E1?style=flat-square&logo=postgresql&logoColor=white) |
| **Test / Quality** | ![JUnit5](https://img.shields.io/badge/JUnit_5-25A162?style=flat-square&logo=junit5&logoColor=white) ![Mockito](https://img.shields.io/badge/Mockito-78A641?style=flat-square) ![SonarQube](https://img.shields.io/badge/SonarQube-4E9BCD?style=flat-square&logo=sonarqube&logoColor=white) |
| **Infra / Tools** | ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white) ![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white) ![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=flat-square&logo=swagger&logoColor=black) |

---

## 🚀 Projects

### 😂 [LOLog](https://github.com/LOLog-2026) · 일상 대화를 4컷만화·쇼츠로 만드는 Android 서비스
`2026.11.02 ~ 2026.11.27 (4주)` · `3인 팀 · AI·인프라 / 앱·화면 디자인 / 백엔드·앱` · **백엔드(api·auth) 단독 개발 + 앱 녹음·STT·기능 개발**

폰에서 바로 글자로 바꾼 대화를 받아, Jetson 3대에서 돌리는 로컬 AI가 웃긴 이야기를 찾아 카드로 모으고
4컷만화·쇼츠 영상으로 만들어 주는 서비스입니다. 무거운 AI 작업을 큐로 넘기고 결과를 받아 정리하는 **백엔드 전체**를 맡았습니다.

- 맡은 기능: 녹음 글 받기 → 이야기 카드(에피소드), 4컷만화·쇼츠, 직접 쓴 글·**블로그 RSS·Atom**(플랫폼별 구현)으로 카드 만들기, 프로필 캐릭터·내 목소리, **친구·방**(초대·좋아요·댓글), **알림함·공지·FCM 푸시**, **관리자**(공지·신고·정지·탈퇴)까지 api·auth에서 구현
- 오래 걸리는 감지·만화·쇼츠는 202로 바로 답하고 **Redis Streams 큐 3개**로 Jetson 워커에 넘김. 쇼츠는 **3호(목소리) → 2호(그림·립싱크)** 두 단계를 백엔드가 이어 줌
- **작업 상태는 DB가 기준**: 큐에 넣기 전에 `jobs` 표에 먼저 적고 1분마다 못 들어간 작업을 다시 넣음, 같은 결과가 두 번 와도 `jobId`로 거름
- 중복 막기: 녹음 id + 묶음 번호로 **멱등 업로드**(다시 보내도 카드는 한 번), 진행 중인 만화·쇼츠는 카드당 하나(**부분 유니크 인덱스**)
- 인증 서버를 따로 두고 게이트웨이 없이 각 서비스가 **JWT를 직접 검사**, 정지·탈퇴는 **Redis 표시로 바로 차단**. 이메일 인증번호 가입, 구글·네이버 로그인
- 파일은 MinIO **10분 서명 주소**로만 줌, 남의 카드·만화는 있는지도 모르게 404
- 앱(Android): 폰 안 STT 모델을 직접 비교해 **SenseVoice-Small** 선정(CER 12%, 1분 음성 약 3초), 백그라운드 녹음·**음성 저장**·업로드
- 앱 기능을 서버에 연결: 다시 듣기, 프로필 캐릭터·내 목소리, 쇼츠, 친구 방, 글·블로그, 관리자, **푸시를 누르면 그 화면으로 이동**
- GitHub Actions로 테스트 → 이미지 → 배포, **헬스체크 실패 시 되돌림**. 테스트 **api 205 · auth 165개**(Testcontainers), 머지 PR **api 50 · auth 20**

🔗 [서비스](https://lolog.java21.net) · [조직](https://github.com/LOLog-2026) · [메인 백엔드](https://github.com/LOLog-2026/LOLog-api) · [인증 서버](https://github.com/LOLog-2026/LOLog-auth) · [명세 문서](https://github.com/LOLog-2026/LOLog-docs) · [소개 쇼츠](https://youtube.com/shorts/Iz_HE9hed28)

### 📝 [now-log](https://github.com/blog-hjiwon203/now-log) · 티스토리형 멀티 블로그 서비스
`2026.10.08 ~ 2026.10.22 (2주)` · `개인 프로젝트` · **명세·설계·백엔드·프론트·배포 전부 혼자**

누구나 가입해 자기 블로그(`/@주소`)를 열고, 에디터로 글을 발행하고, 다른 블로그 글에 댓글·공감·구독하는 서비스를
**명세부터 운영 배포까지** 만들었습니다. 지금도 [now-log.java21.net](https://now-log.java21.net)에서 돌아갑니다.

- [Spec Kit](https://github.com/github/spec-kit)으로 **명세 → 계획 → 작업 목록 163개**를 먼저 쓰고 스텝 단위로 구현. 명세와 달라지면 코드보다 문서를 먼저 고침
- 볼 수 없는 글은 로그인 여부와 상관없이 **404**: 가시성 판단을 `PostVisibilityPolicy` 한 곳에 모아 서버에서만 검사
- 중복 요청 방지를 요청 성격별로 나눔: 공감·구독은 **DB 유니크 제약**, 글 발행·댓글은 **Idempotency-Key**(Redis에 첫 응답 저장)
- 앱이 두 대여도 한 번만: 예약 발행은 **행 잠금**, 새벽 통계 집계는 **PostgreSQL advisory lock**
- 글 저장이 커밋된 뒤 **비동기로 임베딩**(`@TransactionalEventListener` + `@Async`, Ollama bge-m3) → **pgvector**로 의미 검색·비슷한 글 추천
- GitHub Actions로 테스트 → Docker 이미지 → **앱 두 대 롤링 무중단 배포**(readiness 확인, 실패 시 되돌림). 배포 중 요청 362번 모두 200
- 테스트 **백엔드 417개 · 프론트 86개**(Testcontainers, ArchUnit 계층 검사), **SonarQube 품질 게이트 통과**

🔗 [서비스](https://now-log.java21.net) · [코드](https://github.com/blog-hjiwon203/now-log) · [명세·설계 문서](https://github.com/blog-hjiwon203/blog-docs) · [릴리스](https://github.com/blog-hjiwon203/now-log/releases)

### 🏫 [4iren](https://github.com/nhnacademy-aiot3-4iren) · AIoT 지능형 강의실 환경 관제 플랫폼
`2026.07 ~ 2026.09 (11주)` · `NHN Academy AIoT 3기 · 9인 팀` · **Rule Engine 메인 개발자**

강의실 센서(CO2·온습도·TVOC) 데이터가 `MQTT → RabbitMQ`로 흐르는 MSA에서,
관리자가 화면에 노드로 그린 규칙(예: *CO2 1000ppm 초과 → 10분 지속 → 환기 알림*)을 실행하는 **룰 엔진**을 맡았습니다.

- 플로우 그래프 **BFS 실행 엔진**과 OR 병합 노드의 **BLOCKED 전파(DFS)** 설계
- 재시도 단위(센서 메시지)와 **실패 격리 단위(플로우)를 분리**: 3회 재시도 → DLQ, 플로우 실패는 전용 이벤트로 분리
- **Semaphore**로 동시 실행 수 제한, 활성 플로우 일괄 로딩 + Redis 캐싱으로 N+1·DB 부하 제거
- 저장 전 9단계 플로우 무결성 검증(순환 참조 탐지 포함), 오류를 노드별로 모아 응답
- 룰 엔진 저장소 **커밋 91% · PR 23/35** 작성, 테스트 **265개 통과 · 라인 커버리지 85%**

🔗 [룰 엔진 저장소](https://github.com/nhnacademy-aiot3-4iren/4iren-rule-engine) · [위키](https://github.com/nhnacademy-aiot3-4iren/4iren-wiki/wiki/Rule%E2%80%90engine-%E2%80%90%EC%84%B8%EB%B6%80%EA%B8%B0%EB%8A%A5) · [서비스](https://4iren.site)

### 📋 [MiniDooray](https://github.com/hjiwon203/MiniDooray) · 협업 프로젝트 관리 서비스 (MSA)
`2026.05 (9일)` · `4인 팀 · Front / Gateway / Account API / Task API` · **Task API 담당 (2인)**

프로젝트·업무·태그·마일스톤·댓글을 다루는 **Task API** 서버를 팀원 1명과 함께 맡아, 업무(Task)·댓글·태그 도메인을 구현했습니다.

- 업무 생성·수정·삭제 API와 **업무-태그 N:M 매핑**(`TaskTag`): 수정 시 현재 태그와 요청 태그를 비교해 바뀐 것만 추가·제거
- 다른 프로젝트의 태그·마일스톤이 섞이지 않도록 **프로젝트 소속 검증**
- 업무 목록 조회의 **N+1 문제를 fetch join으로 해결** (마일스톤·태그 함께 조회)
- 사용자별 프로젝트 목록을 **상태별 필터 + 최신순 페이징**으로 제공
- `ErrorCode` 기반 **공통 예외 처리**(`GlobalExceptionHandler`)로 에러 응답 형식 통일
- 서비스·리포지토리·컨트롤러 테스트 작성 (JUnit 5, Mockito)

---

## 📊 GitHub Stats

<div align="center">
  <img height="160" src="https://github-readme-stats.vercel.app/api?username=hjiwon203&show_icons=true&include_all_commits=true&count_private=true&hide_border=true" />
  <img height="160" src="https://github-readme-stats.vercel.app/api/top-langs/?username=hjiwon203&layout=compact&hide_border=true" />
</div>

