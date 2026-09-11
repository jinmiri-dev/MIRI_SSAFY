# 진미리 포트폴리오

삼성 청년 SW·AI 아카데미  
(Samsung Software AI Academy For Youth, SSAFY) 15기 부울경 캠퍼스 교육 이수 중

> SSAFY는 삼성전자가 운영하는 SW 인재 양성 교육 프로그램으로,  
> 1년간 집중적인 SW·AI 교육과 실전 프로젝트를 수행합니다.

---

## 교육 과정

| 기수 | 캠퍼스 | 기간 | 과정 | SW 역량 등급 |
|---|---|---|---|---|
| SSAFY 15기 | 부울경 | 2026.01.07 ~ | Python · Web · AI · Django · DB · JavaScript · Vue.js | IM |

> **SW 역량 테스트**는 주어진 문제를 직접 코딩하여 해결하는 실기 방식의 SW 문제해결 역량 평가입니다.

---

## 활동 내역

| 활동 | 내용 | 기간 |
|---|---|---|
| SSAFY 부울경 1반 CA | 자치회 활동 (Class Ambassador) | 2026년 1학기 |
| 광주 캠퍼스 SSAFY MeetUp! 쇼미더아이디어 최우수 🏆 | 개발 언어 선택을 주제로 한 콩트 형식 발표, 5인 팀 참가 및 상금 30만 원 수상 | 2026년 1학기 |
| SSAFY 15기 부울경 1반 관통 프로젝트 최우수 🏆 | AI 영화 추천 서비스 `Pick & Go` 개발 및 팀장 수행 | 2026년 1학기 |
| SSAFY 15기 공통 프로젝트 우수 🏆 | AIoT 주차 관리 서비스 `주차바로` 개발, AI 담당 | 2026년 2학기 |

---

## 프로젝트 목록

| # | 프로젝트 | 유형 | 기간 | 담당 역할 | 기술 스택 | 링크 |
|---|---|---|---|---|---|---|
| 1 | [Pick & Go — 감정과 취향으로 찾는 AI 영화 추천 서비스](./1학기_픽앤고_영화추천사이트) | 1학기 관통 프로젝트 🏆 최우수 | 2026.06.22 ~ 2026.06.26 | 팀장 · AI 추천 기능 개발 | Django 5.2, Vue 3, Python, PostgreSQL, TMDB API, Gemini API | [배포 사이트](https://pick-and-go-2z6d.onrender.com) · [GitHub](https://github.com/jinmiri-dev/pick-and-go) |
| 2 | [주차바로 — AIoT 주차 관리 서비스](#주차바로) | 2학기 공통 프로젝트 🏆 우수 | 2026.07.06 ~ 2026.08.10 | AI 개발 · Edge 디바이스 연동 | YOLOv8, ByteTrack, PaddleOCR, Jetson Orin Nano, Raspberry Pi 5, Spring Boot, React Native | [사용자 서비스](https://i15e202.p.ssafy.io) · [관리자 서비스](https://i15e202.p.ssafy.io/admin/) · [GitLab](https://lab.ssafy.com/s15-webmobile3-sub1/S15P11E202) |

---

## Pick & Go

감정과 취향을 분석하여 사용자에게 영화를 추천하는 AI 영화 추천 커뮤니티 서비스입니다.

- 8개 문항을 기반으로 한 16가지 영화 취향 유형 `MVTI`
- 기분·명대사·취향을 반영한 오늘의 영화 추천
- AI 챗봇을 통한 맞춤형 영화 3편 및 `TOP PICK` 추천
- 친구와의 영화 취향 궁합 분석
- 리뷰·댓글·좋아요 기반 영화 커뮤니티
- 감상한 영화를 기록하는 캘린더

### 프로젝트 성과

- SSAFY 15기 부울경 1반 관통 프로젝트 **최우수상**
- 2인 팀의 팀장으로 서비스 기획·AI 기능·백엔드·배포 수행
- 발표 이후에도 실제 사용 가능한 서비스로 개선하여 배포

<img width="700" alt="관통 프로젝트 최우수상" src="https://github.com/user-attachments/assets/3c273118-1db3-461f-a2ce-df8eabc7e95b" />

### 배포 및 성능 개선

> **배포일: 2026.07.03**

- **인프라**: Render 백엔드·프론트 통합 서빙 + Neon PostgreSQL
- **데이터베이스**: SQLite에서 PostgreSQL로 마이그레이션
- **성능 개선**: N+1 쿼리 최적화
- **AI 모델**: GMS API에서 Gemini API로 전환
- **UI/UX**: 반응형 레이아웃 및 모바일 사용성 개선
- **Render 선택 이유**: Django 상시 구동 서버를 운영할 수 있고, 별도의 카드 등록 없이 무료 배포가 가능하여 선택

---

<a id="주차바로"></a>

## 주차바로

> **주차 문제를 자동으로 감지하고, 대면 없이 차량 이동을 요청하는 AIoT 주차 관리 서비스**

천장 카메라와 입차 카메라를 활용하여 이중주차·주차선 침범 등의 문제를 자동으로 감지하고, 차량 소유자에게 비대면 이동 요청과 알림을 전달합니다.

### 프로젝트 정보

| 구분 | 내용 |
|---|---|
| 프로젝트 유형 | SSAFY 15기 2학기 공통 프로젝트 |
| 트랙 | AIoT |
| 개발 기간 | 2026.07.06 ~ 2026.08.10 (6주) |
| 팀 구성 | 7인 |
| 담당 역할 | AI 개발 및 Edge 디바이스 연동 |
| 수상 | SSAFY 15기 부울경 공통 프로젝트 우수 🏆 |

### 담당 업무

- Jetson Orin Nano 기반 차량 탐지 및 추적 파이프라인 개발
- YOLOv8을 활용한 주차장 내 차량 객체 탐지
- ByteTrack을 활용한 차량별 ID 부여 및 이동 경로 추적
- 차량 위치와 주차 구역의 관계를 분석하여 주차 상태 판정
- 주차선 침범 정도와 차량 이동 방해 정도를 기반으로 불편도 분석
- Raspberry Pi 5 기반 입차 차량 번호판 OCR 파이프라인 개발
- YOLOv8n-pose 기반 번호판 영역 검출
- PaddleOCR을 활용한 차량 번호판 문자 인식
- AI 모델과 백엔드 서버 간 REST 통신 연동
- 실제 주차장 환경을 재현한 AIoT 시연 시스템 구축

### 핵심 기능

- 입차 차량 번호판 자동 인식
- 천장 카메라 기반 차량 탐지 및 실시간 추적
- 이중주차·주차선 침범 등 주차 문제 자동 감지
- 차량 정지 시간과 위치를 활용한 주차 완료 판정
- 차량과 주차 구역의 겹침 비율을 활용한 주차 상태 분석
- 앱을 통한 비대면 차량 이동 요청
- 차량 소유자 대상 실시간 푸시 알림
- 관리자 웹을 통한 주차장 현황 및 발생 사건 관리

### 기술 스택

| 구분 | 기술 |
|---|---|
| Mobile | React Native, Expo, Tailwind CSS |
| Admin Web | Vue 3, Vite |
| Backend | Java 21, Spring Boot 4.1, Spring Security, JWT |
| ORM · Migration | JPA/Hibernate, Flyway |
| Build | Gradle |
| Database | MySQL |
| Jetson AI | YOLOv8, ByteTrack, 주차 상태·불편도 분석 |
| Raspberry Pi AI | YOLOv8n-pose, PaddleOCR |
| Push Notification | Expo Push, FCM, APNs |
| Edge Communication | REST over Tailscale |
| Command Processing | Transactional Outbox Pattern |
| Infra | Docker, Jenkins, AWS EC2, Nginx, Tailscale |
| Edge Device | NVIDIA Jetson Orin Nano, Raspberry Pi 5 |

### 시스템 구성

1. Raspberry Pi가 입차 차량을 촬영하고 번호판을 인식합니다.
2. Jetson이 천장 카메라 영상에서 차량을 탐지하고 추적합니다.
3. 차량 위치·정지 시간·주차 구역 침범 비율을 분석합니다.
4. 주차 문제가 발생하면 백엔드 서버에 사건을 전달합니다.
5. 차량 소유자에게 앱 푸시 알림과 이동 요청을 전송합니다.
6. 관리자는 웹에서 주차 현황과 처리 상태를 확인합니다.

### 프로젝트 성과

- SSAFY 15기 부울경 공통 프로젝트 **우수상**
- YOLOv8·ByteTrack·PaddleOCR을 활용한 AIoT 파이프라인 구현
- Jetson·Raspberry Pi·백엔드 서버를 연결한 실제 동작 시스템 구축
- AI 분석 결과를 모바일 앱의 이동 요청 및 푸시 알림으로 연결

<img width="700" alt="SSAFY 15기 공통 프로젝트 우수상" src="주차바로_상장_이미지_URL" />

### 서비스 및 저장소

- [사용자 서비스](https://i15e202.p.ssafy.io)
- [관리자 서비스](https://i15e202.p.ssafy.io/admin/)
- [프로젝트 GitLab](https://lab.ssafy.com/s15-webmobile3-sub1/S15P11E202)
