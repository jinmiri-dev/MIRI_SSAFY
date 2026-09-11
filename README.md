# 진미리 포트폴리오

삼성 청년 SW AI 아카데미 (Samsung SW AI Academy For Youth, SSAFY) 15기 부울경 캠퍼스 교육 이수 中

> SSAFY는 삼성전자가 운영하는 SW 인재 양성 교육 프로그램으로, 비전공자도 지원 가능하며 1년간 집중적인 SW 교육을 제공합니다.

## 교육 과정

| 기수 | 캠퍼스 | 기간 | 과정 | SW 역량 등급 |
|------|--------|------|------|------------|
| SSAFY 15기 | 부울경 | 2026.01.07 ~ | Python · Web · AI · Django · DB · JavaScript · Vue.js | IM |

> **SW 역량 테스트**란 소프트웨어 직군에 지원한 분들을 대상으로 SW 문제해결 역량을 측정하기 위해 도입되었으며, 주어진 문제에 대한 답안을 직접 코딩하여 문제를 해결하는 실기 방식의 테스트입니다.

---

## 활동 내역

| 활동 | 내용 | 기간 |
|------|------|------|
| SSAFY 부울경 1반 CA | 자치회 활동 (Class Ambassador) | 2026 1학기 |
| 광주 캠퍼스 SSAFY MeetUp! 쇼미더아이디어 최우수 🏆 | 상금 30만원 - 개발 언어 선택을 주제로 한 콩트 형식 발표 (5인 1팀으로 참가) | 2026 1학기 |
| SSAFY 15기 부울경 1반 관통 프로젝트 최우수 🏆 | AI 영화 추천 서비스 'Pick & Go' 개발 (팀장) | 2026 1학기 |
| SSAFY 15기 공통 프로젝트 우수 🏆 | AIoT 주차 관리 서비스 '주차바로' 개발 (AI 담당) | 2026 2학기 |

---

## 프로젝트 목록

| # | 프로젝트 README | 유형 | 기간 | 기술 스택 | 배포 링크 |
|---|---------|------|------|---------|------|
| 1 | [Pick & Go — 감정과 취향으로 찾는 AI 영화 추천 서비스](./1학기_픽앤고_영화추천사이트) | 1학기 관통 프로젝트 🏆 최우수 | 2026.06.22 ~ 06.26 | Django 5.2, Vue 3, Python, SQLite, TMDB API, GMS API | [🔗 배포 사이트](https://pick-and-go-2z6d.onrender.com) · [💻 GitHub](https://github.com/jinmiri-dev/pick-and-go) |
| 2 | [주차바로 — AIoT 주차 관리 서비스](./2학기_공통프로젝트_주차바로) | 2학기 공통 프로젝트 🏆 우수 | 2026.07.06 ~ 08.10 | YOLOv8, ByteTrack, PaddleOCR, Jetson Orin Nano, Raspberry Pi, Spring Boot, React Native | [👤 사용자](https://i15e202.p.ssafy.io) · [🛠️ 관리자](https://i15e202.p.ssafy.io/admin/) · [💻 GitLab](https://lab.ssafy.com/s15-webmobile3-sub1/S15P11E202) |

---

### 1학기 관통 프로젝트 최우수상

<img width="1080" height="1527" alt="관통프로젝트_최우수상" src="https://github.com/user-attachments/assets/3c273118-1db3-461f-a2ce-df8eabc7e95b" />
<img width="2429" height="3275" alt="공통프로젝트_우수상" src="https://github.com/user-attachments/assets/a4faebb7-bb8b-4bb6-9e4a-07c906653c9b" />


> **🚀 Pick & Go 배포 (2026.07.03)**
>
> 발표 이후에도 실제 접속 가능한 서비스로 완성하고자 무료 배포를 진행했습니다.
>
> - **인프라**: Render(백엔드+프론트 통합 서빙) + Neon PostgreSQL
> - **주요 작업**: SQLite→PostgreSQL 마이그레이션 · N+1 쿼리 최적화 · GMS→Gemini API 전환 · 반응형/모바일 UX 개선
> - **Render 선택 이유**: Vercel(서버리스)은 Django 상시 구동 서버와 궁합이 안 맞고, Railway/Fly.io는 무료 티어가 사라져 카드 등록이 필요해짐

---

### 2학기 공통 프로젝트 우수상

<!-- 아래 주소를 GitHub에 상장 사진을 올린 후 생성되는 주소로 교체하세요. -->
<img width="1080" alt="공통프로젝트_우수상" src="여기에_상장_이미지_URL_입력" />

> **🚗 주차바로**
>
> 주차 문제를 자동으로 감지하고, 대면 없이 차량 이동을 요청하는 AIoT 주차 관리 서비스입니다.
>
> - **프로젝트 유형**: SSAFY 15기 2학기 공통 프로젝트
> - **트랙**: AIoT
> - **기간**: 2026.07.06 ~ 2026.08.10 (6주)
> - **담당 역할**: AI 개발 및 Edge 디바이스 연동
> - **수상**: SSAFY 15기 부울경 공통 프로젝트 우수 🏆

#### 주요 기능

- 입차 차량의 번호판 자동 검출 및 OCR 인식
- 천장 카메라 기반 차량 탐지 및 실시간 추적
- 주차 완료 여부와 주차 상태 자동 판정
- 주차선 침범 및 이동 방해 정도 분석
- 앱을 통한 비대면 차량 이동 요청
- 사용자 대상 실시간 푸시 알림
- 관리자 웹을 통한 주차 현황 및 사건 관리

#### 기술 스택

- **Frontend**
  - 사용자 앱: React Native (Expo), Tailwind CSS
  - 관리자 웹: Vue 3 (Vite)

- **Backend**
  - Java 21
  - Spring Boot 4.1
  - Spring Security, JWT
  - JPA/Hibernate, Flyway
  - Gradle

- **Database**
  - MySQL

- **AI / Edge**
  - Jetson: YOLOv8 차량 탐지, ByteTrack 추적, 주차 상태 및 불편도 분석
  - Raspberry Pi: YOLOv8n-pose 번호판 검출, PaddleOCR 번호판 인식

- **통신 / 알림**
  - Expo Push
  - Android FCM / iOS APNs
  - Edge ↔ Server: REST over Tailscale 사설망
  - Backend → Edge 명령: 아웃박스 패턴

- **Infrastructure**
  - Docker
  - Jenkins
  - AWS EC2
  - Nginx
  - Tailscale

#### 서비스 링크

- [👤 사용자 서비스](https://i15e202.p.ssafy.io)
- [🛠️ 관리자 서비스](https://i15e202.p.ssafy.io/admin/)
- [💻 프로젝트 GitLab](https://lab.ssafy.com/s15-webmobile3-sub1/S15P11E202)
