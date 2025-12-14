# ☕ Cafind

> **카페 실시간 좌석·웨이팅 관리 플랫폼**
> 헛걸음 없는 카페 이용, 스마트한 매장 운영

Cafind는 **모바일 사용자와 카페 운영자 모두를 위한 실시간 좌석·웨이팅 관리 서비스**입니다.
Android / iOS 모바일 앱과 NestJS 기반 백엔드 서버를 중심으로,
**실제 카페 운영 환경을 가정한 End-to-End 서비스 구조**를 설계·구현했습니다.

---

🔗 [Frontend README 보기](https://github.com/Cafind/cafind/tree/main/frontend#readme)

🔗 [Backend README 보기](https://github.com/Cafind/cafind/tree/main/backend#readme)

---

## 📋 목차

- [프로젝트 개요](#-프로젝트-개요)
- [기획 배경 및 문제 정의](#-기획-배경-및-문제-정의)
- [핵심 가치](#-핵심-가치)
- [전체 시스템 아키텍처](#-전체-시스템-아키텍처)
- [주요 기능](#-주요-기능)
- [기술 스택](#-기술-스택)
- [데이터 흐름](#-데이터-흐름)
- [시연 영상](#-시연-영상)
- [팀 구성 및 역할](#-팀-구성-및-역할)
- [프로젝트 구조](#-프로젝트-구조)
- [실행 방법 (Quick Start)](#-실행-방법-Quick-Start)
- [트러블슈팅 & 기술적 고민](#-트러블슈팅-및-기술적-고민)
- [향후 발전 계획](#-향후-발전-계획)

---

## 📌 프로젝트 개요

### 서비스 한 줄 소개

> **"실시간 좌석과 대기 현황을 한눈에 확인하는 카페 이용 플랫폼"**

Cafind는 사용자가 방문 전에 **카페의 혼잡도와 좌석 상태를 실시간으로 확인**하고,
운영자는 **좌석·웨이팅을 디지털로 관리**할 수 있도록 돕는 서비스입니다.

본 프로젝트는 단순 CRUD 앱이 아닌,

- 실시간 상태 동기화
- 다중 사용자 동시 접근
- 운영자/사용자 역할 분리

를 고려한 **실서비스 지향 구조**를 목표로 개발되었습니다.

---

## 💡 기획 배경 및 문제 정의

### 기존 문제점

- 카페 방문 전 **좌석 유무를 알 수 없음**
- 인기 매장의 경우 **현장 웨이팅 관리의 비효율성**
- 전화·구두 안내 중심의 대기 관리로 인한 혼선

### Cafind의 해결 방식

- 좌석 상태를 **실시간 데이터로 시각화**
- 모바일 앱을 통한 **비대면 웨이팅 등록 및 호출**
- 운영자용 관리 화면을 통한 **좌석 회전율 개선**

---

## 🎯 핵심 가치

- **Real-time**
  WebSocket(Socket.IO)을 활용하여 좌석·웨이팅 상태를 즉시 동기화

- **Reliability**
  PostgreSQL 트랜잭션 + Redis 캐싱을 통한 안정적인 상태 관리

- **Scalability**
  모듈화된 NestJS 구조와 타입 안전한 Prisma ORM 기반 확장성 확보

- **User Experience**
  사용자·운영자 각각의 동선을 고려한 UX 설계

---

## 🏗 전체 시스템 아키텍처

```text
┌──────────────────────────┐
│     Mobile Application   │
│  React Native + Expo     │
└───────────┬──────────────┘
            │ REST API
            │ WebSocket
            ▼
┌────────────────────────────────┐
│        NestJS API Server       │
│  - Auth / Users / Cafes        │
│  - Seats / Waitlist            │
│  - Realtime Gateway            │
└───────────┬───────────┬────────┘
            │           │
            │ ORM       │ Cache
            ▼           ▼
┌──────────────────┐  ┌──────────────────┐
│   PostgreSQL     │  │      Redis       │
│ (Prisma ORM)     │  │ (Cache / PubSub) │
└──────────────────┘  └──────────────────┘
```

---

## ✨ 주요 기능

### 👤 사용자 기능

- 위치 기반 주변 카페 탐색
- 카페 상세 정보 및 좌석 배치도 확인
- 좌석별 실시간 이용 가능 여부 확인
- 원격 웨이팅 등록 및 호출 알림 수신

### 🧑‍💼 운영자 기능

- 좌석 체크인 / 체크아웃 관리
- 웨이팅 목록 관리 및 고객 호출
- 실시간 좌석·대기 현황 모니터링

### ⚡ 실시간 기능

- 좌석 상태 변경 시 즉시 UI 반영
- 웨이팅 호출 시 사용자 알림 전송

---

## 🛠 기술 스택

### Frontend

| 분류             | 기술             | 설명                                      |
| ---------------- | ---------------- | ----------------------------------------- |
| Framework        | React Native     | iOS / Android 크로스플랫폼 모바일 앱 개발 |
| Platform         | Expo             | 개발 환경 통합 및 빌드/배포 편의성 제공   |
| Language         | TypeScript       | 정적 타입 기반 안정성 확보                |
| Routing          | Expo Router      | 파일 기반 라우팅 구조                     |
| State Management | Zustand          | 전역 상태 관리 (유저, 웨이팅 등)          |
| Data Fetching    | Axios            | REST API 통신                             |
| Real-time        | Socket.IO Client | 실시간 좌석·웨이팅 이벤트 수신            |

### Backend

| 분류           | 기술       | 설명                               |
| -------------- | ---------- | ---------------------------------- |
| Framework      | NestJS     | 모듈화된 서버 아키텍처 (Monorepo)  |
| Language       | TypeScript | 타입 안정성과 유지보수성 향상      |
| ORM            | Prisma     | 타입 안전한 DB 접근 및 스키마 관리 |
| Database       | PostgreSQL | 트랜잭션 기반 안정적인 데이터 저장 |
| Cache / PubSub | Redis      | 캐싱 및 실시간 이벤트 보조         |
| Real-time      | Socket.IO  | WebSocket 기반 실시간 통신         |

### Infra

| 분류          | 기술           | 설명                       |
| ------------- | -------------- | -------------------------- |
| Container     | Docker         | 개발/운영 환경 일관성 유지 |
| Orchestration | Docker Compose | DB 및 Redis 컨테이너 관리  |

---

## 🔄 데이터 흐름

1. 앱 진입 시 REST API로 초기 데이터 조회
2. Socket.IO 연결 및 카페 채널 구독
3. 좌석/웨이팅 변경 이벤트 발생
4. 서버 → 클라이언트 실시간 이벤트 전송
5. 클라이언트 상태 업데이트 및 UI 즉시 반영

---

## 🎥 시연 영상

https://github.com/user-attachments/assets/1e8e7a5e-6b20-47df-bf75-fe8fb7fe917d

---

## 👥 팀 구성 및 역할

| 역할               | 이름   | 담당 업무                         |
| ------------------ | ------ | --------------------------------- |
| Backend            | 김우준 | API 설계 및 DB 모델링             |
| Backend            | 최건희 | 실시간 시스템 및 API 개발         |
| Frontend / Backend | 홍성환 | UX 개발, 전체 구조 설계, API 연동 |
| Frontend           | 안태환 | UX 기획 및 디자인                 |

---

## 📁 프로젝트 구조

```bash
/
├── frontend/   # React Native (Expo)
├── backend/    # NestJS API Server
└── README.md   # Project Overview
```

각 디렉토리 내부에는 세부 README가 포함되어 있습니다.

---

## 🚀 실행 방법 (Quick Start)

### 1. 저장소 클론

```bash
git clone https://github.com/your-org/cafind.git
cd cafind
```

### 2. 백엔드 실행

```bash
cd backend
cp .env.example .env
docker-compose up -d
npm install
npm run prisma:migrate
npm run start:dev
```

### 3. 프론트엔드 실행

```bash
cd frontend
npm install
npx expo start
```

---

## 🧠 트러블슈팅 & 기술적 고민

**실시간 동기화 문제**

- REST + WebSocket 역할을 분리하여 초기 로딩과 실시간 반영을 분리

**중복 웨이팅 방지**

- 서버 단에서 상태 검증 및 트랜잭션 처리

**좌석 상태 일관성**

- 체크인/체크아웃 로직을 단일 진입점으로 통합

---

## 🔮 향후 발전 계획

- FCM 기반 푸시 알림 연동
- 매출 및 이용 통계 자동화
- 운영자 웹 대시보드 제공
- PostGIS 도입을 통한 위치 검색 최적화
- 서비스 권한 관리 고도화 (RBAC)

---

## 📄 라이선스

MIT License
