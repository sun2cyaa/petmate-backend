# 🐾 PetMate Backend

> 반려동물 라이프스타일 통합 예약 플랫폼 백엔드 서버

PetMate는 반려동물을 위한 다양한 서비스(미용, 호텔, 병원 등)를 한 곳에서 검색하고 예약할 수 있는 통합 플랫폼입니다.

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.5-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![MySQL](https://img.shields.io/badge/MySQL-8.0-blue.svg)](https://www.mysql.com/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## 📋 목차

- [주요 기능](#-주요-기능)
- [기술 스택](#-기술-스택)
- [프로젝트 구조](#-프로젝트-구조)
- [시작하기](#-시작하기)
- [API 명세](#-api-명세)
- [환경 변수 설정](#-환경-변수-설정)
- [배포](#-배포)

## ✨ 주요 기능

### 👥 회원 관리
- 소셜 로그인 (Google, Kakao, Naver)
- JWT 기반 인증/인가
- 회원 정보 관리
- 프로필 이미지 업로드

### 🐕 반려동물 관리
- 반려동물 등록 및 정보 관리
- 반려동물 프로필 이미지 업로드
- 품종 정보 관리

### 🏢 업체(펫메이트) 관리
- 업체 등록 및 정보 관리
- 사업자 등록번호 검증 (국세청 API 연동)
- 운영 시간 및 서비스 정보 관리
- 업체 이미지 업로드

### 📅 예약 관리
- 실시간 예약 가능 시간 조회
- 예약 생성 및 관리
- 예약 상태 관리
- 예약 내역 조회

### 🛍️ 상품/서비스 관리
- 서비스 카테고리별 상품 관리
  - 미용 서비스
  - 호텔 서비스
  - 병원 서비스
  - (확장 가능: 장례, 유치원 등)
- 상품 검색 및 필터링
- 가용 시간대 관리

### 💳 결제 관리
- 결제 정보 등록
- 결제 내역 조회
- 결제 상태 관리

### ⭐ 리뷰 관리
- 리뷰 작성 및 조회
- 리뷰 키워드 관리
- 평점 관리

### 📍 지도 기반 검색
- 카카오맵 API 연동
- 위치 기반 업체 검색
- 거리 계산 기능

## 🛠 기술 스택

### Backend
- **Framework**: Spring Boot 3.5.5
- **Language**: Java 21
- **Build Tool**: Gradle
- **ORM**: 
  - JPA (Hibernate) - 주요 데이터 처리
  - MyBatis 3.0.4 - 복잡한 쿼리 처리

### Database
- **RDBMS**: MySQL 8.0

### Security
- **Authentication**: Spring Security + OAuth2 Client
- **Authorization**: JWT (JSON Web Token)
- **Social Login**: Google, Kakao, Naver

### Storage
- **File Storage**: AWS S3
- **Local Storage**: Local file system (개발 환경)

### DevOps
- **Containerization**: Docker
- **CI/CD**: (추가 가능)

### External APIs
- **Map Service**: Kakao Map API

## 📁 프로젝트 구조

```
src/main/java/com/petmate/
├── PetmateApplication.java          # 메인 애플리케이션
├── common/                           # 공통 모듈
│   ├── entity/                       # 공통 엔티티 (BaseEntity, CodeEntity, ImageEntity)
│   ├── repository/                   # 공통 리포지토리
│   ├── service/                      # 공통 서비스 (ImageService)
│   ├── storage/                      # 파일 저장 서비스
│   └── util/                         # 유틸리티 클래스
├── config/                           # 설정 클래스
│   ├── JpaConfig.java
│   ├── MyBatisConfig.java
│   ├── SecurityConfig.java
│   ├── S3Config.java
│   └── WebConfig.java
├── domain/                           # 도메인별 모듈
│   ├── address/                      # 주소 관리
│   ├── auth/                         # 인증/인가
│   ├── booking/                      # 예약 관리
│   ├── company/                      # 업체 관리
│   ├── payment/                      # 결제 관리
│   ├── pet/                          # 반려동물 관리
│   ├── product/                      # 상품 관리
│   ├── review/                       # 리뷰 관리
│   └── user/                         # 회원 관리
├── security/                         # 보안 관련
│   ├── CustomOAuth2UserService.java
│   ├── CustomUserDetails.java
│   ├── JwtAuthenticationFilter.java
│   ├── OAuth2SuccessHandler.java
│   └── jwt/                          # JWT 유틸리티
└── global/                           # 전역 설정
    └── ApiExceptionHandler.java      # 전역 예외 처리

src/main/resources/
├── application.yml                   # 애플리케이션 설정
├── mybatis/
│   ├── config/
│   │   └── mybatis-config.xml       # MyBatis 설정
│   └── mappers/                      # MyBatis Mapper XML
│       ├── booking/
│       ├── payment/
│       ├── product/
│       ├── review/
│       ├── token/
│       └── user/
└── static/
    └── profiles/                     # 기본 프로필 이미지
```

### 도메인 구조 (각 도메인 동일)
```
domain/{domain-name}/
├── controller/      # REST API 컨트롤러
├── dto/
│   ├── request/     # 요청 DTO
│   └── response/    # 응답 DTO
├── entity/          # JPA 엔티티
├── repository/
│   ├── jpa/         # JPA 리포지토리
│   └── mybatis/     # MyBatis 매퍼 (선택적)
└── service/         # 비즈니스 로직
```

## 🚀 시작하기

### 사전 요구사항
- Java 21 이상
- MySQL 8.0 이상
- Gradle 8.x
- AWS S3 계정 (파일 저장용)
- OAuth2 클라이언트 ID/Secret (Google, Kakao, Naver)

### 설치 및 실행

1. **저장소 클론**
```bash
git clone https://github.com/sun2cyaa/petmate-backend.git
cd petmate-backend
```

2. **환경 변수 설정**

프로젝트 루트에 `.env` 파일 생성:
```env
# Database
DB_URL=jdbc:mysql://localhost:3306/petmate?useSSL=false&serverTimezone=Asia/Seoul
DB_USERNAME=your_username
DB_PASSWORD=your_password

# JWT
JWT_SECRET=your_jwt_secret_key
JWT_ISSUER=petmate
JWT_EXPIRATION=3600000
JWT_REFRESH_EXPIRATION=604800000

# OAuth2
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
KAKAO_CLIENT_ID=your_kakao_client_id
KAKAO_CLIENT_SECRET=your_kakao_client_secret
NAVER_CLIENT_ID=your_naver_client_id
NAVER_CLIENT_SECRET=your_naver_client_secret

# AWS S3
AWS_S3_BUCKET=your_bucket_name
AWS_S3_ACCESS_KEY=your_access_key
AWS_S3_SECRET_KEY=your_secret_key
AWS_S3_REGION=ap-northeast-2

# Application
REACT_APP_SPRING_API_BASE=http://localhost:8090
REACT_APP_FRONT_BASE_URL=http://localhost:3000
UPLOAD_ROOT_DIR=/path/to/upload/directory
```

3. **데이터베이스 생성**
```sql
CREATE DATABASE petmate DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

4. **애플리케이션 실행**

**Gradle 사용:**
```bash
./gradlew bootRun
```

**JAR 빌드 및 실행:**
```bash
./gradlew build
java -jar build/libs/petmate-0.0.1-SNAPSHOT.jar
```

5. **Docker 사용 (선택사항)**
```bash
docker build -t petmate-backend .
docker run -p 8090:8090 --env-file .env petmate-backend
```

서버가 `http://localhost:8090`에서 실행됩니다.

## 📚 API 명세

### 인증 (Auth) - `/auth`
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/auth/signout` | 로그아웃 (refresh 쿠키 제거) | ❌ |
| POST | `/auth/refresh` | Access Token 갱신 | ❌ (쿠키) |
| GET | `/auth/me` | 내 정보 조회 | ✅ |
| POST | `/auth/signout-all` | 모든 기기에서 로그아웃 | ✅ |
| POST | `/auth/admin/cleanup-tokens` | 만료된 토큰 수동 정리 (관리자용) | ✅ |
| GET | `/oauth2/redirect` | OAuth2 리다이렉트 엔드포인트 | ❌ |

### 회원 (User) - `/user`
| Method | Endpoint | Description | Content-Type |
|--------|----------|-------------|--------------|
| POST | `/user/apply` | 기본 유저 등록 | application/x-www-form-urlencoded |
| POST | `/user/petmate/apply` | 펫메이트 신청 | multipart/form-data |
| POST | `/user/petowner/apply` | 반려인 신청 | multipart/form-data |
| POST | `/user/profile/apply` | 프로필 등록/수정 | multipart/form-data |
| PUT | `/user/me` | 내 프로필 수정 | multipart/form-data |
| DELETE | `/user/me` | 회원 탈퇴 | - |
| POST | `/user/restore` | 탈퇴 계정 복구 | application/json |

### 반려동물 (Pet) - `/pet`
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/pet/my` | 내 반려동물 목록 조회 | ✅ |
| GET | `/pet/my/{petId}` | 내 특정 반려동물 조회 | ✅ |
| POST | `/pet/apply` | 반려동물 등록 | ✅ |
| GET | `/pet/breeds?species={code}` | 품종 목록 조회 (D:개, C:고양이 등) | ❌ |
| PUT | `/pet/{petId}` | 반려동물 정보 수정 | ✅ |
| DELETE | `/pet/{petId}` | 반려동물 삭제 | ✅ |
| PATCH | `/pet/{petId}/image` | 이미지 URL 직접 갱신 | ✅ |
| POST | `/pet/{petId}/image` | 이미지 파일 업로드 | ✅ (multipart) |

### 업체 (Company) - `/api/company`
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/company/register` | 업체 등록 | ✅ |
| GET | `/api/company/my` | 내가 등록한 업체 목록 | ✅ |
| GET | `/api/company/{id}` | 업체 상세 조회 | ✅ |
| GET | `/api/company/public/{id}` | 공개 업체 정보 조회 (예약용) | ❌ |
| PUT | `/api/company/{id}` | 업체 정보 수정 | ✅ |
| DELETE | `/api/company/{id}` | 업체 삭제 | ✅ |
| PUT | `/api/company/{id}/status` | 업체 승인 상태 변경 | ✅ |
| POST | `/api/company/get-business-info` | 사업자등록번호 조회 및 검증 | ❌ |
| GET | `/api/company/check-personal-exists` | 개인 업체 중복 확인 | ✅ |
| GET | `/api/company/nearby` | 주변 업체 조회 (위도/경도 기반) | ❌ |
| GET | `/api/company/{id}/service-types` | 업체별 제공 서비스 유형 조회 | ❌ |

**주변 업체 조회 파라미터:**
- `latitude`: 위도 (필수)
- `longitude`: 경도 (필수)
- `radius`: 반경 (km, 기본값: 5.0)
- `serviceType`: 서비스 타입 필터 (선택)
- `keyword`: 검색 키워드 (선택)

### 상품 (Product) - `/api/products`
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/products` | 전체 상품 목록 조회 | ✅ |
| GET | `/api/products/companies` | 내 업체 목록 (상품 등록용) | ✅ |
| POST | `/api/products` | 상품 등록 | ✅ |
| GET | `/api/products/{id}` | 상품 단건 조회 | ❌ |
| GET | `/api/products/search` | 상품 검색 (필터링) | ❌ |
| GET | `/api/products/company/{companyId}` | 업체별 상품 목록 조회 | ❌ |
| PUT | `/api/products/{id}` | 상품 정보 수정 | ✅ |
| GET | `/api/products/{id}/deletion-check` | 상품 삭제 전 확인 (예약 여부) | ✅ |
| DELETE | `/api/products/{id}` | 상품 삭제 (슬롯 포함) | ✅ |
| GET | `/api/products/service-categories` | 서비스 카테고리 목록 | ❌ |

**서비스 카테고리:**
- `C`: 돌봄
- `W`: 산책
- `G`: 미용
- `H`: 병원
- `E`: 기타

### 예약 (Booking) - `/api/booking`
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/booking` | 예약 생성 | ✅ |
| GET | `/api/booking/{id}` | 예약 상세 조회 | ✅ |
| GET | `/api/booking/user/{userId}` | 사용자별 예약 목록 조회 | ✅ |
| GET | `/api/booking/company/{companyId}` | 업체별 예약 목록 조회 | ✅ |
| POST | `/api/booking/{id}/status` | 예약 상태 변경 (0:대기, 1:확정, 2:완료, 3:취소) | ✅ |
| PUT | `/api/booking/{id}/cancel` | 예약 취소 (사용자용) | ✅ |
| PUT | `/api/booking/{id}/confirm` | 예약 확정 (업체용) | ✅ |
| GET | `/api/booking/payment/{id}` | 결제용 예약 정보 조회 | ❌ |
| DELETE | `/api/booking/payment-failed/{id}` | 결제 실패로 인한 예약 삭제 | ❌ |

### 예약 가능 시간 (TimeSlot) - `/api/products/{productId}`
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/products/{productId}/available-slots` | 예약 가능 시간 조회 | ❌ |
| POST | `/api/products/{productId}/refresh-slots` | 시간 슬롯 새로고침 | ❌ |

**파라미터:**
- `date`: 조회할 날짜 (YYYY-MM-DD 형식)

### 리뷰 (Review) - `/api/reviews`
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/reviews` | 리뷰 작성 | ✅ |
| GET | `/api/reviews/reservation/{reservationId}` | 특정 예약의 내 리뷰 조회 | ✅ |
| GET | `/api/reviews/my` | 내 리뷰 목록 조회 | ✅ |
| DELETE | `/api/reviews/{reviewId}` | 리뷰 삭제 (소유자만) | ✅ |
| GET | `/api/reviews/company/{companyId}` | 업체별 리뷰 목록 조회 (공개) | ❌ |

**파라미터:**
- `companyId`: 업체 ID 필터 (선택)
- `page`: 페이지 번호 (기본값: 0)
- `size`: 페이지 크기 (기본값: 20)

### 결제 (Payment) - `/api/payment`
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/payment/process` | 결제 처리 | ❌ |
| GET | `/api/payment/{paymentId}` | 결제 상세 조회 | ❌ |
| GET | `/api/payment/reservation/{reservationId}` | 예약별 결제 내역 조회 | ❌ |
| POST | `/api/payment/{paymentId}/cancel` | 결제 취소 | ❌ |
| GET | `/api/payment/status/{orderId}` | 결제 상태 조회 | ❌ |
| GET | `/api/payment/health` | 헬스 체크 | ❌ |

**다날 결제 콜백:**
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET/POST | `/api/payment/danal/success` | 다날 결제 성공 콜백 |
| GET/POST | `/api/payment/danal/fail` | 다날 결제 실패 콜백 |

### 주소 (Address) - `/api/address`
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/address` | 주소 목록 조회 | ✅ |
| GET | `/api/address/{id}` | 기본 주소 조회 | ✅ |
| POST | `/api/address` | 주소 등록 | ✅ |
| PUT | `/api/address/{id}` | 주소 수정 | ✅ |
| DELETE | `/api/address/{id}` | 주소 삭제 | ✅ |
| PUT | `/api/address/{id}/default` | 기본 주소 설정 | ✅ |

**주소 목록 조회 파라미터:**
- `userLat`: 사용자 위도 (선택, 거리 계산용)
- `userLng`: 사용자 경도 (선택, 거리 계산용)

### 파일 업로드
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/files/upload` | 파일 업로드 (S3/로컬) |
| GET | `/img/{filename}` | 이미지 조회 (프록시) |

### 인증 관련
- **JWT 기반 인증**: Authorization 헤더에 `Bearer {token}` 형식으로 전송
- **OAuth2 로그인**: `/login/oauth2/code/{provider}` (Google, Kakao, Naver)
- **리프레시 토큰**: HttpOnly 쿠키로 관리 (7일 유효)

상세한 Request/Response 스키마는 각 도메인의 DTO 클래스를 참고하세요.

## 🔧 환경 변수 설정

### 필수 환경 변수

#### Database
- `DB_URL`: MySQL 데이터베이스 URL
- `DB_USERNAME`: 데이터베이스 사용자명
- `DB_PASSWORD`: 데이터베이스 비밀번호

#### JWT
- `JWT_SECRET`: JWT 시크릿 키 (최소 256bit)
- `JWT_ISSUER`: JWT 발급자
- `JWT_EXPIRATION`: 액세스 토큰 만료 시간 (밀리초)
- `JWT_REFRESH_EXPIRATION`: 리프레시 토큰 만료 시간 (밀리초)

#### OAuth2
- `GOOGLE_CLIENT_ID`: Google OAuth2 클라이언트 ID
- `GOOGLE_CLIENT_SECRET`: Google OAuth2 클라이언트 시크릿
- `KAKAO_CLIENT_ID`: Kakao OAuth2 클라이언트 ID
- `KAKAO_CLIENT_SECRET`: Kakao OAuth2 클라이언트 시크릿
- `NAVER_CLIENT_ID`: Naver OAuth2 클라이언트 ID
- `NAVER_CLIENT_SECRET`: Naver OAuth2 클라이언트 시크릿

#### AWS S3
- `AWS_S3_BUCKET`: S3 버킷 이름
- `AWS_S3_ACCESS_KEY`: AWS 액세스 키
- `AWS_S3_SECRET_KEY`: AWS 시크릿 키
- `AWS_S3_REGION`: AWS 리전 (예: ap-northeast-2)

#### Application
- `REACT_APP_SPRING_API_BASE`: 백엔드 API Base URL
- `REACT_APP_FRONT_BASE_URL`: 프론트엔드 Base URL
- `UPLOAD_ROOT_DIR`: 파일 업로드 디렉토리 경로

## 🏗️ 아키텍처

### 레이어드 아키텍처
```
┌─────────────────────────────────────┐
│         Presentation Layer          │
│         (Controllers)               │
├─────────────────────────────────────┤
│         Business Layer              │
│         (Services)                  │
├─────────────────────────────────────┤
│         Persistence Layer           │
│    (Repositories - JPA/MyBatis)     │
├─────────────────────────────────────┤
│         Database Layer              │
│         (MySQL)                     │
└─────────────────────────────────────┘
```

### 주요 설계 패턴
- **Layered Architecture**: 계층별 명확한 책임 분리
- **DTO Pattern**: 계층 간 데이터 전달
- **Repository Pattern**: 데이터 접근 추상화
- **Factory Pattern**: 객체 생성 로직 캡슐화
- **Strategy Pattern**: 파일 저장 전략 (Local/S3)

### 보안 아키텍처
```
┌─────────────┐
│   Client    │
└──────┬──────┘
       │ Request + JWT
       ▼
┌─────────────────────────────────┐
│  JwtAuthenticationFilter        │
│  (JWT 검증 및 인증)              │
└──────┬──────────────────────────┘
       │ Authentication
       ▼
┌─────────────────────────────────┐
│  SecurityContextHolder          │
│  (보안 컨텍스트 저장)            │
└──────┬──────────────────────────┘
       │
       ▼
┌─────────────────────────────────┐
│  Controller                     │
│  (@PreAuthorize 권한 체크)       │
└─────────────────────────────────┘
```

## 📦 배포

### Docker를 사용한 배포

프로젝트에 포함된 Dockerfile을 사용하여 컨테이너화된 환경에서 실행할 수 있습니다.

**Dockerfile 특징:**
- Multi-stage build 사용
- Eclipse Temurin JDK 21 Alpine 기반
- 빌드 단계와 실행 단계 분리로 이미지 크기 최적화

**1. Docker 이미지 빌드**
```bash
docker build -t petmate-backend:latest .
```

**2. Docker 컨테이너 실행**
```bash
docker run -d \
  -p 8090:8090 \
  --name petmate-backend \
  --env-file .env \
  petmate-backend:latest
```

**3. 환경 변수 전달**
```bash
# 또는 개별 환경 변수로 실행
docker run -d \
  -p 8090:8090 \
  --name petmate-backend \
  -e DB_URL=jdbc:mysql://host.docker.internal:3306/petmate \
  -e DB_USERNAME=your_username \
  -e DB_PASSWORD=your_password \
  -e JWT_SECRET=your_secret \
  petmate-backend:latest
```

**4. 로그 확인**
```bash
docker logs -f petmate-backend
```

## 🔍 로깅

애플리케이션의 모든 로그는 `logs/petmate.log` 파일에 저장됩니다.

**로깅 설정:**
- **로그 레벨**: DEBUG (개발 환경)
- **로그 파일**: `logs/petmate.log`
- **로그 로테이션**: 10MB 단위
- **보관 기간**: 30일

**로그 확인:**
```bash
# 실시간 로그 확인
tail -f logs/petmate.log

# 에러 로그만 확인
tail -f logs/petmate.log | grep ERROR
```

---
