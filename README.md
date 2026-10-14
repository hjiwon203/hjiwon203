<div align="center">

# 황지원(Hwang Ji-won) | Backend Developer

**실패가 번지지 않는 서버를 만듭니다.**<br/>
비동기 메시지 파이프라인에서 실패를 격리하고, 사용자가 만든 규칙을 안전하게 실행하는 엔진을 설계·구현했습니다.

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
| **Messaging / Cache** | ![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white) |
| **Database** | ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white) |
| **Test / Quality** | ![JUnit5](https://img.shields.io/badge/JUnit_5-25A162?style=flat-square&logo=junit5&logoColor=white) ![Mockito](https://img.shields.io/badge/Mockito-78A641?style=flat-square) ![SonarQube](https://img.shields.io/badge/SonarQube-4E9BCD?style=flat-square&logo=sonarqube&logoColor=white) |
| **Infra / Tools** | ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white) ![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white) ![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=flat-square&logo=swagger&logoColor=black) |

---

## 🚀 Projects

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

