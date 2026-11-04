# WalkingBeans (배달 앱)

![Java](https://img.shields.io/badge/Java%2021-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot%203.4-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![MyBatis](https://img.shields.io/badge/MyBatis-007ACC?style=for-the-badge&logo=mybatis&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Gradle](https://img.shields.io/badge/Gradle-02303A?style=for-the-badge&logo=gradle&logoColor=white)
![React](https://img.shields.io/badge/React%2018-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Bootstrap](https://img.shields.io/badge/Bootstrap-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white)
![Lombok](https://img.shields.io/badge/Project%20Lombok-FF2D20?style=for-the-badge&logo=lombok&logoColor=white)

## 프로젝트 개요

**WalkingBeans**는 고객(User) · 가게 사장(Owner) · 라이더(Rider) · 관리자(Admin) 네 가지 역할을 지원하는 **배달 주문 웹 애플리케이션**입니다.

- 고객은 지도 기반으로 주변 가게를 찾고, 메뉴/옵션을 장바구니에 담아 **토스페이먼츠**로 결제한 뒤 배달 현황을 실시간으로 확인합니다.
- 사장은 가게 정보·메뉴·옵션을 관리하고, 들어온 주문을 접수·조리 상태로 변경하며 매출을 확인합니다.
- 라이더는 **카카오맵** 위에서 배차 대기 주문을 확인해 배달을 수락하고, 픽업 → 배달 완료까지 진행하며 수입을 조회합니다.
- 주문 상태 변경·알림·채팅은 **WebSocket(STOMP / SockJS)** 으로 실시간 전달됩니다.

프론트엔드(React)와 백엔드(Spring Boot REST API)가 분리된 구조이며, 백엔드는 MyBatis로 MySQL과 통신합니다.

- **개발 기간**: 2025.02.18 ~ 2025.04.08
- **화면 설계**: [Figma – 워킹 빈즈 프로젝트 전체 view](https://www.figma.com/design/r7ymgmCcU0TBja0hoypXVF/%EC%9B%8C%ED%82%B9-%EB%B9%88%EC%A6%88-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%A0%84%EC%B2%B4-view?node-id=0-1)
- **ERD**: [ERD Cloud – walkingbeans](https://www.erdcloud.com/d/AzTuGWv9LCEjvyXfp)
- **일정 관리**: [GitHub – walking-beans](https://github.com/walking-beans)
- **프로젝트 문서**: [Notion – Walking Beans](https://foremost-relish-ca5.notion.site/Walking-Beans-1d078a4ac00981bfb065deb31475350b)

---

## 주요 기술 스택

### Backend
| 구분 | 기술 |
|------|------|
| Language | Java 21 |
| Framework | Spring Boot 3.4.2 (Web, Security, WebSocket, AOP, Mail, Validation, Actuator) |
| Persistence | MyBatis 3.0.4, MySQL (mysql-connector-j) |
| Build | Gradle |
| 기타 | Lombok, BCrypt 비밀번호 암호화, springdoc-openapi (Swagger UI) |

### Frontend
| 구분 | 기술 |
|------|------|
| Library | React 18, React Router v7 |
| 통신 | Axios, @stomp/stompjs, sockjs-client |
| UI | Bootstrap 5, styled-components |
| 지도 | react-kakao-maps-sdk (Kakao Maps API) |
| 결제 | @tosspayments/tosspayments-sdk |

### 외부 API
- **Kakao / Naver OAuth** – 소셜 로그인
- **Kakao Maps** – 가게·배달지 지도 표시, 주소 검색
- **Toss Payments** – 결제 요청 및 승인
- **Imgur API** – 이미지 업로드
- **Gmail SMTP** – 이메일 인증 코드 발송

### 개발 환경
| 구분 | 내용 |
|------|------|
| Tool | IntelliJ IDEA, MySQL Workbench, Postman |
| 형상관리 | Git, GitHub (GitHub Desktop, 코드 리뷰) |
| 협업 | Notion (문서화), Figma (화면 설계), ERD Cloud (DB 설계) |

---

## 주요 기능

### 👤 고객 (User)
- 이메일 로그인 / 카카오·네이버 소셜 로그인, 이메일 인증 코드 기반 본인 확인
- 지도 기반 주변 가게 검색, 대표 주소 기준 10km 이내 매장 키워드 검색
- 매장 상세 페이지 카테고리별 메뉴 조회, 메뉴 옵션 선택
- 장바구니 담기 / 수량 변경 / 삭제 (가게 단위 관리)
- 배송지 등록 / 대표(기본) 배송지 설정
- 신용카드 결제(토스페이먼츠, 성공 / 실패 페이지 처리) / 만나서 결제
- 주문 내역(주문 상태별 버튼 표시) · 주문 상세 조회/삭제 · 실시간 배달 현황 조회
- 가게 리뷰 작성 / 수정 / 삭제, 라이더 평점
- 마이페이지 (프로필 이미지 업로드, 정보 수정, 회원 탈퇴), 역할 변경(사장/라이더 신청)

### 🏪 가게 사장 (Owner)
- 가게 신규 등록 및 기본·운영·배달·위치 정보 수정
- 가게 상태 변경 (운영 중 / 준비 중 / 휴무일), 배달 시간 설정
- 메뉴 등록·수정·약한 삭제(Soft Delete)·복구, 메뉴 옵션 대량 등록·개별 삭제
- 실시간 주문 조회 및 주문 상태 변경 (주문 수락 → 조리 중 → 조리 완료), 조리 시간 조정
- 매출 조회, 가게 알림 로그
- `@OwnershipCheck` AOP로 본인 가게에 대한 요청만 허용 (타 가게 수정 제한)

### 🛵 라이더 (Rider)
- 카카오맵에 라이더 현재 위치 표시, '운행 중' 상태일 때 반경 10km 내 수락 가능 주문의 매장을 마커로 표시
- 매장별 주문 목록 확인 및 배달 수락
- 배달 중 3초 단위 위치 갱신, 픽업 / 배달 완료 처리, 고객 위치 제공
- 배달 내역 및 월별 수입(DeliveryIncome) 조회

### 🛠 관리자 (Admin)
- 사용자 역할 관리, 아이디 로그인 제한
- 전체 공지 알림 발송
- 채팅방 / 메시지 관리

### 🔔 실시간 기능
- 주문 상태 변경 알림, 채팅 알림, 읽음/안읽음 알림 관리
- 라이더 배달 수락 시 고객·매장과의 채팅방 자동 생성
- 사용자 ↔ 관리자 / 라이더 ↔ 고객·매장 채팅

### 주문 상태 코드
| 코드 | 상태 |
|:---:|------|
| 2 | 주문 요청 |
| 3 | 주문 조리 중 |
| 4 | 조리 완료 |
| 5 | 라이더 픽업 완료 |
| 6 | 배달 완료 |

---

## DB 설계

ERD: [ERD Cloud – walkingbeans](https://www.erdcloud.com/d/AzTuGWv9LCEjvyXfp)

| 주요 사용 테이블 | 기능 |
|------------------|------|
| users | 회원가입 / 로그인 / 마이페이지 / 관리자 기능 |
| stores | 매장 조회 (지도 기반) |
| stores, reviews, menu, menuOption, carts | 매장 상세 보기 / 메뉴 보기 |
| reviews, riderReviews | 리뷰 작성 / 삭제 |
| orders, orderItems, payments | 주문, 결제 |
| chattingroom, chattingMember, message | 채팅 |
| deliveryIncome | 라이더 수입 |
| alarm | 알림 |

---

## 디렉토리 구조

```
WalkingBeans
├── walking-beans-backend                     # Spring Boot REST API 서버
│   ├── build.gradle
│   └── src/main
│       ├── java/walking_beans/walking_beans_backend
│       │   ├── WalkingBeansBackendApplication.java
│       │   ├── aspect        # OwnershipCheck / OwnershipAspect (가게 소유권 검증 AOP)
│       │   ├── config        # CORS, DB, Security, Mail, WebConfig, LoginInterceptor,
│       │   │                 # WebSocket(주문·알림·채팅) 설정 및 핸들러
│       │   ├── controller    # REST / STOMP 컨트롤러
│       │   ├── mapper        # MyBatis Mapper 인터페이스
│       │   ├── model
│       │   │   ├── dto       # Users, Stores, Menu, MenuOption, Carts, Orders, Payments,
│       │   │   │             # Reviews, RiderReview, Address, Alarms, ChattingRoom, Message ...
│       │   │   │   ├── admin # UserChattingRoom, UserMessage
│       │   │   │   ├── order # OrderRequest, OrderItems, OrderDetailDTO, UserCartDTO ...
│       │   │   │   └── rider # RiderMainStoreInfo, RiderOrderStatusDTO
│       │   │   └── vo        # DeliveryStatus, Vertification
│       │   └── service       # 도메인별 Service 인터페이스 + ServiceImpl
│       └── resources
│           ├── application.properties
│           ├── config.properties   # DB·API 키 등 로컬 설정 (gitignore 대상)
│           ├── mybatis-config.xml
│           └── mappers/*.xml        # SQL 매퍼
│
└── walking-beans-frontend                    # React SPA
    ├── package.json                # proxy: http://localhost:7070
    ├── public
    └── src
        ├── index.jsx
        ├── components              # 라우팅(PathRoute, ProtectedRoute), 공통/역할별 컴포넌트
        │   ├── admin               # 헤더 알림, 알림 전송
        │   ├── owner               # 메뉴 카드, 옵션 폼, 주문 카드, 가게 정보 폼
        │   └── rider               # 라이더 지도, 주문 상태
        ├── pages
        │   ├── admin               # 로그인, 마이페이지, 역할 변경, 채팅, 알림 목록
        │   ├── user                # 가게/메뉴, 장바구니, 결제, 주문 내역, 리뷰, 배송지
        │   ├── owner               # 가게 관리, 메뉴, 주문, 매출
        │   ├── rider               # 메인 지도, 배달 중, 완료, 내역, 수입
        │   └── layout              # 역할별 헤더, 검색, 에러 페이지
        ├── service                 # Axios API 모듈
        └── css
```

---

## API 개요

| 도메인 | Base URL | 설명 |
|--------|----------|------|
| 사용자 | `/api/users` | 로그인/로그아웃, 세션, 이메일 인증, 마이페이지, 역할 변경, 탈퇴 |
| 소셜 로그인 | `/oauth/kakao/*`, `/oauth/naver/*` | 카카오·네이버 OAuth 로그인 및 콜백 |
| 주소 | `/api/addresses` | 배송지 등록·조회·삭제, 기본 배송지 설정 |
| 가게 | `/api/store` | 가게 조회·수정, 검색, 주변 가게, 라이더 메인 정보 |
| 메뉴 | `/api/menu` | 메뉴 조회·검색, 사장 메뉴 등록·수정·삭제 |
| 메뉴 옵션 | `/api/option` | 옵션 조회·수정·삭제 |
| 장바구니 | `/api/cart` | 장바구니 조회·추가·수정·삭제 |
| 주문 | `/api/orders`, `/api/order` | 주문 생성·조회·삭제, 상태 변경, 라이더 배차 |
| 결제 | `/api/payment` | 토스 결제 요청·승인, 결제 수단 조회 |
| 리뷰 | `/api/reviews`, `/api/riderReview` | 가게 리뷰 CRUD, 라이더 평점 |
| 배달 수입 | `/api/deliveryIncome` | 라이더 수입 조회, 배달 완료 처리 |
| 알림 | `/api/alarm`, `/api/read`, `/api/noreadalarms` ... | 알림 조회·읽음·삭제, 공지 발송 |
| 채팅 | `/api/chatting*`, `/api/chattingroom`, `/api/message` | 채팅방·메시지 조회 및 생성 |

**WebSocket 엔드포인트**: `/ws`, `/ws-order`, `/ws-alarm` (STOMP + SockJS), `/ws/chat`, `/ws/alert`, `/ws/orders`, `/ws/chatting`

---

## 팀원 소개

<table>
  <tr>
    <th>김경문</th>
    <th>김서현</th>
    <th>이도형</th>
    <th>박현진</th>
    <th>이성호</th>
    <th>황아현</th>
  </tr>
  <tr>
    <td><!-- 역할 --></td>
    <td><!-- 역할 --></td>
    <td><!-- 역할 --></td>
    <td><!-- 역할 --></td>
    <td>라이더 웹페이지<br>채팅 구현</td>
    <td><!-- 역할 --></td>
  </tr>
</table>

### 이성호 담당 기능

#### 🛵 라이더 웹페이지
- **메인 페이지**
  - 라이더 현재 위치를 카카오맵에 표시
  - '운행 중' 상태일 때 라이더 위치 반경 10km 내 수락 가능한 주문을 맵에 마커로 표시
  - 주문 수락 시 해당 주문 Update 및 고객·매장과의 채팅방 Create
- **배달 페이지**
  - 라이더 현재 위치를 3초 단위로 갱신해 맵에 표시
  - 주문 상태를 확인해 '픽업 중' / '배달 중' 상태 표시
  - 매장과 고객 위치를 맵에 표시, 픽업 시 매장 마커 삭제 후 배달 중 상태로 변경
  - 배달 완료 시 배달 완료 페이지로 이동
  - 해당 주문에 대한 권한이 없으면 메인 페이지로 redirect
- **배달 완료 페이지**
  - 주문 정보 표시, 권한이 없으면 메인 페이지로 redirect
- **수입 관리 페이지**
  - 오늘 날짜 기준 이번 달 수입을 먼저 보여주고, 사용자 선택에 따라 월별 수입 조회
- **주문 내역 페이지**
  - 배달 중 / 배달 완료 여부에 따라 완료 시간순 정렬
  - 배달 중이면 배달 페이지로, 배달 완료면 배달 완료 페이지로 이동

#### 💬 채팅 기능
- **채팅 목록**
  - 사용자의 `user_role`에 따라 채팅 목록을 분류하고 마지막 메시지 시간순으로 정렬 (예: 라이더는 고객 / 매장 목록을 나눠 정렬)
  - `WITH` 절로 chattingroom · chattingmember · message · users를 조인해 상대방 기준 마지막 메시지를 표시
- **채팅방**
  - 먼저 작성된 메시지부터 위에서 아래로 렌더링, 본인 메시지는 우측 / 상대 메시지는 좌측 표시
  - 채팅방 · 내 채팅 목록 · 상대 채팅 목록이 모두 WebSocket에 연결되어, 메시지 작성 시 연결된 모든 화면에 즉시 전송

### 개선 예정
- 카카오 내비게이션 연동 (라이더 길 안내)
- 채팅 이미지 첨부 및 화면 렌더링

---

## 실행 방법

### 사전 요구 사항
- JDK 21
- Node.js (npm)
- MySQL

### 1. 저장소 클론
```bash
git clone https://github.com/sarden0302/WalkingBeans.git
cd WalkingBeans
```

### 2. 백엔드 설정
`walking-beans-backend/src/main/resources/config.properties` 파일을 생성하고 아래 항목을 채웁니다.

```properties
server.port=7070

spring.datasource.hikari.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.hikari.jdbc-url=jdbc:mysql://localhost:3306/<DB명>
spring.datasource.hikari.username=<DB 사용자>
spring.datasource.hikari.password=<DB 비밀번호>

mybatis.mapper-locations=classpath:mappers/*.xml

upload-img=C://beans-image        # 업로드 이미지 저장 경로
toss.secretKey=<토스페이먼츠 시크릿 키>
imgur.client.id=<Imgur Client ID>
```

### 3. 백엔드 실행
```bash
cd walking-beans-backend
./gradlew bootRun        # Windows: gradlew.bat bootRun
```
서버는 [http://localhost:7070](http://localhost:7070)에서 실행됩니다.

### 4. 프론트엔드 설정 및 실행
`walking-beans-frontend/.env` 파일에 카카오맵 키를 설정합니다. (라이더 지도 화면에서 사용)
```env
REACT_APP_KAKAO_MAP_API_KEY_LEO=<카카오 JavaScript 키>
```

```bash
cd walking-beans-frontend
npm install
npm start
```
브라우저에서 [http://localhost:3000](http://localhost:3000)을 열어 확인합니다.

---

## 문의

프로젝트와 관련하여 문의 사항이 있으면 아래 이메일로 연락주세요.

이성호 Email: [sarden030@gmail.com](mailto:sarden030@gmail.com)
