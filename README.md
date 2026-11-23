<p align="center">
  <img src="assets/images/banner.svg" width="100%" alt="SSAFY 15기 포트폴리오 배너" />
</p>

<p align="center">
  SSAFY 15기에서 진행한 네 개의 프로젝트를 모았습니다.<br />
  프로젝트마다 <b>팀 README</b>, <b>내가 한 일과 선택의 이유</b>, <b>코드를 다시 읽은 벨로그 기록</b>이 있습니다.
</p>

<p align="center">
  <a href="https://github.com/jinmiri-dev">👋 프로필 · 수상 · 상장</a> &nbsp;·&nbsp;
  <a href="https://velog.io/@bara0121">📝 velog</a>
</p>

---

## 📂 프로젝트

| 프로젝트 | 기간 | 결과 | 나의 역할 | 자료 |
|---|---|---|---|---|
| 🎬 **Pick & Go**<br /><sub>AI 영화 추천 웹 서비스</sub> | 1학기 관통<br /><sub>2026.06.22 ~ 06.26</sub> | 🥇 최우수 | 팀장 · 프론트엔드 · 백엔드 · UI/UX | [README](./1학기_픽앤고_영화추천사이트) · [내가 한 일](./1학기_픽앤고_영화추천사이트/내가_한_일.md) · [velog](https://velog.io/@bara0121/series/%EA%B4%80%ED%86%B5-AI-%EC%98%81%ED%99%94-%EC%B6%94%EC%B2%9C-%EC%84%9C%EB%B9%84%EC%8A%A4) · [서비스](https://pick-and-go-2z6d.onrender.com) · [GitHub](https://github.com/jinmiri-dev/pick-and-go) |
| 🚗 **주차바로**<br /><sub>AIoT 주차 관리 서비스</sub> | 2학기 공통<br /><sub>2026.07.06 ~ 08.10</sub> | 🥈 우수 | AI · 엣지 디바이스 연동 | [README](./2학기_주차바로_AIoT) · [velog](https://velog.io/@bara0121/series/%EC%98%81%EC%83%81-AI-%EC%8B%A4%EC%A0%84-%EB%85%B8%ED%8A%B8) · [GitLab](https://lab.ssafy.com/s15-webmobile3-sub1/S15P11E202) |
| 🌉 **GABOLLE(가볼래)**<br /><sub>부산 초개인화 여행 추천 앱</sub> | 2학기 특화<br /><sub>2026.08.18 ~ 09.28</sub> | 🥈 우수 · 반 2등 | 웹·앱 프론트엔드 | [README](./2학기_가볼래_빅데이터_분산) · [내가 한 일](./2학기_가볼래_빅데이터_분산/내가_한_일.md) · [velog](https://velog.io/@bara0121/series/%ED%8A%B9%ED%99%94-%EB%B6%80%EC%82%B0-%EC%97%AC%ED%96%89-%EC%B6%94%EC%B2%9C-%EC%95%B1) · [Google Play](https://lnkd.in/gu9tqb-W) · [App Store](https://lnkd.in/ghwvukiK) |
| 🍳 **쿠킹테이블**<br /><sub>요리 영상 레시피 AI 서비스 · 기업연계</sub> | 2학기 자율<br /><sub>2026.10.12 ~ 11.20</sub> | 🎤 발표 완료<br /><sub>결과 발표 전</sub> | 인프라 · 로그인 서비스 · 앱 화면 | [README](./2학기_쿠킹테이블__기업연계) · [내가 한 일](./2학기_쿠킹테이블__기업연계/내가_한_일.md) · [velog](https://velog.io/@bara0121/series/%EC%9E%90%EC%9C%A8-%EC%9A%94%EB%A6%AC-%EC%98%81%EC%83%81-%EB%A0%88%EC%8B%9C%ED%94%BC-%EC%84%9C%EB%B9%84%EC%8A%A4) |

> **내가 한 일** 문서에는 프로젝트에서 부딪힌 문제마다 고민한 선택지(A안·B안·C안), 고른 이유, 결과를 적었습니다.

---

### 🎬 Pick & Go · 관통 프로젝트 최우수

<img src="assets/images/pickandgo.svg" width="360" align="right" alt="Pick & Go" />

현재 감정과 영화 취향 유형인 **MVTI**를 바탕으로 영화를 추천하고, 감상 기록과 커뮤니티 활동까지 연결하는 **개인 맞춤형 영화 추천 서비스**입니다.

- **역할**: 팀장 · 프론트엔드 · 백엔드 · UI/UX. 전체 화면과 사용자 흐름을 설계하고, Vue 화면과 Django API를 함께 만들었습니다. 발표 뒤 직접 무료 배포했습니다.
- **기술**: `Django 5.2` `Vue 3` `Python` `PostgreSQL` `TMDB API` `Gemini API` `Render`
- **선택 사례**: AI 점수를 실시간 대신 MVTI 유형별 배치로 · 목록 N+1 쿼리 제거 · AI 검사 실패 시 통과(fail-open) · 무료 호스팅으로 Render 선택

<br clear="right" />

### 🚗 주차바로 · 공통 프로젝트 우수

<img src="assets/images/jucha-baro.svg" width="360" align="right" alt="주차바로" />

주차 문제를 자동으로 감지하고, 대면 없이 차량 이동을 요청하는 **AIoT 주차 관리 서비스**입니다.

- **역할**: AI 개발 및 엣지 디바이스 연동
- **기술**: `YOLOv8` `ByteTrack` `PaddleOCR` `Jetson Orin Nano` `Raspberry Pi` `Spring Boot` `React Native`
- **주요 기능**: 번호판 자동 검출·OCR, 천장 카메라 차량 추적, 주차 완료 판정, 주차선 침범·이중주차 판정

<br clear="right" />

### 🌉 GABOLLE(가볼래) · 특화 프로젝트 우수

<img src="assets/images/gabolle.svg" width="360" align="right" alt="GABOLLE" />

여행 목적·취향·예산·이동 조건을 반영해 부산 여행지를 추천하고 하루 일정으로 구성해 주는 **초개인화 로컬 여행 서비스**입니다. Google Play와 App Store에 출시했습니다.

- **역할**: 프론트엔드. React Native(Expo) 한 코드로 앱과 웹 화면을 만들고, 화면에 필요한 API 일부도 만들었습니다. Jira 담당 이슈 376개.
- **기술**: `React Native` `Expo` `TypeScript` `Spring Boot` `PostgreSQL` `Python` `OR-Tools`
- **선택 사례**: 긴 추천은 웹 SSE·앱 폴링 · 동시 401에 갱신은 한 번(single-flight) · 숫자가 낀 문구 208곳을 "모양"으로 번역

<br clear="right" />

### 🍳 쿠킹테이블 · 자율 프로젝트(기업연계)

<img src="assets/images/cookingtable.svg" width="360" align="right" alt="쿠킹테이블" />

요리 영상을 올리면 AI가 단계별 레시피로 나눠 주고, 학습에 동의한 영상은 **조리 로봇 기업의 학습 데이터**가 되는 서비스입니다.

- **역할**: 인프라 담당. 팀 서버 네트워크·HTTPS, 로그인 서비스(Spring Boot, OAuth·JWT), 보안 점검과 강화, 암호화 백업과 복구 연습, 앱 화면 일부. Jira 담당 작업 83개.
- **기술**: `Nginx` `Ubuntu` `AWS Lightsail` `Spring Boot` `PostgreSQL` `React Native` `Qwen VLM`
- **선택 사례**: 업로드 경로만 따로 크게 · 로그인은 한 서비스 + RS256/JWKS · 앱엔 1회용 code만 · 백업은 암호화 + 매주 복구 연습 · SSH 변경 전 자동 되돌리기

<br clear="right" />

---

<p align="center">
  <a href="https://github.com/jinmiri-dev">👋 프로필로 돌아가기</a> &nbsp;·&nbsp;
  <a href="https://velog.io/@bara0121">📝 velog</a>
</p>
