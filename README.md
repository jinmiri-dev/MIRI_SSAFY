# SSAFY 15기 프로젝트 포트폴리오

> **진미리** · 우사기처럼 즐겁게 협업하는 Product Engineer
>
> SSAFY 15기에서 진행한 프로젝트와 활동, 수상 내역을 정리했습니다.


---


## 활동 내역


| # | 활동 | 내용 |
|------|------|------|
| 1 | SSAFY 부울경 1반 CA | 자치회 활동 (Class Ambassador) |
| 2 | 광주 캠퍼스 SSAFY MeetUp! 쇼미더아이디어 최우수 🏆 | 상금 30만원 - 개발 언어 선택을 주제로 한 콩트 형식 발표 (5인 1팀으로 참가) |
| 3 | SSAFY 15기 부울경 1반 관통 프로젝트 최우수 🏆 | AI 영화 추천 서비스 'Pick & Go' 개발 (팀장) |
| 4 | SSAFY 15기 공통 프로젝트 우수 🏆 | AIoT 주차 관리 서비스 '주차바로' 개발 (AI 담당) |
| 5 | SSAFY 15기 특화 프로젝트 우수 🏆 | 개인의 취향에 맞춘 여행 추천 서비스 'GABOLLE(가볼래)' 개발 (프론트엔드 담당) |


---


## 프로젝트 목록


| # | 프로젝트 README | 유형 | 기간 | 기술 스택 | 배포 링크 |
|---|---------|------|------|---------|------|
| 1 | [Pick & Go — 감정과 취향으로 찾는 AI 영화 추천 서비스](./1학기_픽앤고_영화추천사이트) | 1학기 관통 프로젝트 🏆 최우수 | 2026.06.22 ~ 06.26 | Django 5.2, Vue 3, Python, SQLite, TMDB API, GMS API | [🔗 배포 사이트](https://pick-and-go-2z6d.onrender.com) · [💻 GitHub](https://github.com/jinmiri-dev/pick-and-go) |
| 2 | [주차바로 — AIoT 주차 관리 서비스](./2학기_공통프로젝트_주차바로) | 2학기 공통 프로젝트 🏆 우수 | 2026.07.06 ~ 08.10 (6주) | YOLOv8, ByteTrack, PaddleOCR, Jetson Orin Nano, Raspberry Pi, Spring Boot, React Native | [💻 GitLab](https://lab.ssafy.com/s15-webmobile3-sub1/S15P11E202) |
| 3 | [GABOLLE(가볼래) — 부산 초개인화 여행 추천 서비스](./2학기_가볼래_빅데이터_분산) | 2학기 특화 프로젝트 🏆 우수 | 2026년 2학기 | React, TypeScript, React Native, Spring Boot, PostgreSQL, Redis, Python, OR-Tools | [🤖 Google Play](https://lnkd.in/gu9tqb-W) · [🍎 App Store](https://lnkd.in/ghwvukiK) · [💻 GitHub](https://github.com/jinmiri-dev/gabolle_bigdata) |


---


### 1학기 관통 프로젝트 최우수상


> **🎬 Pick & Go**
>
> 현재 감정과 영화 취향 유형인 **MVTI**를 바탕으로 영화를 추천하고, 감상 기록과 커뮤니티 활동까지 연결하는 **개인 맞춤형 영화 추천 서비스**입니다.
>
> - **담당 역할**: 팀장 · 프론트엔드 · UI/UX · 데이터 구성. 전체 화면 디자인과 사용자 흐름을 설계하고, MVTI 결과·영화 달력·커뮤니티·배지·알림·픽키 챗봇 UI를 구현했습니다.
>
> #### 주요 기능
> - 8문항 영화 취향 테스트를 통한 16가지 MVTI 유형 분석과 결과 카드 제공
> - 오늘의 감정·명대사·MVTI를 반영한 AI 영화 추천과 추천 이유 안내
> - 영화 탐색, 장르 필터, 랜덤 영화 뽑기, TOP10 개인화 랭킹
> - 친구 취향 궁합과 함께 보기 좋은 영화 추천
> - 리뷰·댓글·좋아요·알림·영화 달력을 통한 감상 기록 및 커뮤니티 경험


> **🚀 Pick & Go 배포 (2026.07.03)**
>
> 발표 이후에도 실제 접속 가능한 서비스로 완성하고자 무료 배포를 진행했습니다.
>
> - **인프라**: Render(백엔드+프론트 통합 서빙) + Neon PostgreSQL
> - **주요 작업**: SQLite→PostgreSQL 마이그레이션 · N+1 쿼리 최적화 · GMS→Gemini API 전환 · 반응형/모바일 UX 개선
> - **Render 선택 이유**: Vercel(서버리스)은 Django 상시 구동 서버와 궁합이 안 맞고, Railway/Fly.io는 무료 티어가 사라져 카드 등록이 필요해짐


<img width="1080" height="1527" alt="관통프로젝트_최우수상" src="https://github.com/user-attachments/assets/3c273118-1db3-461f-a2ce-df8eabc7e95b" />


---


### 2학기 공통 프로젝트 우수상


> **🚗 주차바로**
>
> 주차 문제를 자동으로 감지하고, 대면 없이 차량 이동을 요청하는 **AIoT 주차 관리 서비스**입니다.
> - **담당 역할**: AI 개발 및 Edge 디바이스 연동
>
> #### 주요 기능
> - 입차 차량의 번호판 자동 검출 및 OCR 인식
> - 천장 카메라 기반 차량 탐지 및 실시간 추적
> - 주차 완료 여부와 주차 상태 자동 판정
> - 주차선 침범 및 이동 방해 정도 분석

![2학기 공통 프로젝트 우수상](./assets/certificates/진미리_SSAFY_공통프로젝트_AIoT_우수상.jpg)
