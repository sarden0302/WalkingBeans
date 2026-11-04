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

---

## 주요 기능

### 👤 고객 (User)
- 이메일 로그인 / 카카오·네이버 소셜 로그인, 이메일 인증 코드 기반 본인 확인
- 지도 기반 주변 가게 검색, 가게·메뉴 검색
- 메뉴 옵션 선택 및 장바구니 (가게 단위 관리)
- 배송지 등록 / 기본 배송지 설정
- 토스페이먼츠 결제 (성공 / 실패 페이지 처리)
- 주문 내역 · 주문 상세 · 실시간 배달 현황 조회
- 가게 리뷰 작성 / 수정 / 삭제, 라이더 평점
- 마이페이지 (프로필 이미지 업로드, 정보 수정, 회원 탈퇴), 역할 변경(사장/라이더 신청)

### 🏪 가게 사장 (Owner)
- 가게 등록 및 기본·운영·배달·위치 정보 관리, 영업 상태/배달 시간 설정
- 메뉴 등록·수정·삭제, 메뉴 옵션 그룹 관리
- 주문 목록 확인 및 주문 상태 변경 (접수 → 조리 중 → 조리 완료)
- 매출 조회, 가게 알림 로그
- `@OwnershipCheck` AOP로 본인 가게에 대한 요청만 허용

### 🛵 라이더 (Rider)
- 카카오맵에서 주변 미배차 주문 확인 및 배달 수락
- 픽업 / 배달 완료 처리, 배달 남은 시간 확인
- 배달 내역 및 수입(DeliveryIncome) 조회

### 🛠 관리자 (Admin)
- 사용자 역할 관리, 전체 공지 알림 발송
- 1:1 채팅방 / 메시지 관리

### 🔔 실시간 기능
- 주문 상태 변경 알림, 읽음/안읽음 알림 관리
- 사용자 ↔ 관리자 채팅

### 주문 상태 코드
| 코드 | 상태 |
|:---:|------|
| 2 | 주문 요청 |
| 3 | 주문 조리 중 |
| 4 | 조리 완료 |
| 5 | 라이더 픽업 완료 |
| 6 | 배달 완료 |

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
    <td><!-- 역할 --></td>
  </tr>
</table>

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
