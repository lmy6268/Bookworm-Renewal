# 📚 책볼레 (Bookworm)

**책볼레(Bookworm)**는 독서 기록을 체계적으로 관리하고, 다른 사용자들과 독서 경험을 공유할 수 있는 안드로이드 커뮤니티 애플리케이션입니다.
책 검색부터 독후감 작성, 독서 통계 제공은 물론 사용자 간 피드 공유와 챌린지, 채팅 기능까지 제공하여 독서의 즐거움을 배가시켜줍니다.

<br>

## 🏛️ 아키텍처 및 디자인 패턴

이 프로젝트는 유지보수성과 확장성을 높이기 위해 **MVVM (Model-View-ViewModel)** 아키텍처 패턴을 기반으로 구현되었습니다.

- **View (UI Layer)**:
  - XML 레이아웃과 `Fragment`, `Activity`로 구성됩니다.
  - `DataBinding`과 `ViewBinding`을 사용하여 UI와 데이터를 효율적으로 결합하고 보일러플레이트 코드를 줄였습니다.
- **ViewModel**:
  - `FeedViewModel`, `ChallengeViewModel`, `SearchViewModel` 등 각 도메인 영역별로 ViewModel이 분리되어 있습니다.
  - `Coroutine`과 `viewModelScope`를 사용하여 비동기 데이터 처리 및 UI 상태 관리를 수행합니다.
- **Model (Repository / Data Layer)**:
  - `FeedDataRepository`, `SearchDataRepository` 등 Repository 패턴을 도입하여 네트워크(API) 및 로컬 데이터베이스의 비즈니스 로직을 뷰모델로부터 분리했습니다.
  - 단일 진실 공급원(Single Source of Truth) 원칙을 지향하여 데이터 흐름을 일관되게 관리합니다.

<br>

## 🛠️ 주요 사용 API 및 라이브러리

### 1. Open APIs 및 서버 측 연동

- **[Aladin Open API](https://www.aladin.co.kr/ttb/wapi_search.aspx)**: 앱 내 도서 검색 및 상세 책 정보(메타데이터, 표지 이미지 등)를 불러오기 위해 사용되었습니다.
- **[Algolia Search API](https://www.algolia.com/)**: 빠르고 강력한 앱 내 데이터(사용자, 피드, 챌린지 등) 검색 기능을 구현하기 위해 사용되었습니다.
- **[Firebase](https://firebase.google.com/)**:
  - `Firebase Auth`: 사용자 인증 (이메일 로그인 등)
  - `Firestore / Realtime Database`: NoSQL 기반 앱 내 클라우드 데이터 저장소 (게시글, 채팅, 챌린지, 기록 등)
  - `Firebase Storage`: 사용자 프로필, 피드 업로드 이미지 등 파일 저장
  - `FCM (Firebase Cloud Messaging)`: 사용자 간 채팅 알림, 피드 알림 등의 푸시 알림
  - `Firebase Dynamic Links`: 특정 앱 내 페이지로 바로 이동할 수 있는 딥링크 공유 기능
  - `Firebase Analytics`: 사용자 행동 및 이벤트 데이터 트래킹

### 2. 소셜 로그인 (Authentication)

- **[Kakao Login SDK](https://developers.kakao.com/docs/latest/ko/kakaologin/android)**: 카카오 계정을 활용한 간편 로그인 지원
- **[Google Play Services Auth](https://developers.google.com/identity/sign-in/android/start-integrating)**: 구글 계정을 활용한 간편 로그인 지원

### 3. 네트워킹 및 비동기 처리

- **[Retrofit2 & Gson](https://square.github.io/retrofit/)**: Aladin API 및 외부 REST API와의 효율적인 HTTP 통신 및 JSON 직렬화/역직렬화
- **Kotlin Coroutines**: 스레드를 블로킹하지 않는 안전하고 효율적인 비동기 UI/네트워크 로직 처리

### 4. UI 및 UX 관련 라이브러리

- **[Glide](https://github.com/bumptech/glide)**: 빠르고 부드러운 이미지 로딩 및 캐싱 처리
- **[uCrop](https://github.com/Yalantis/uCrop)**: 사용자 프로필 사진이나 피드 사진 업로드 전 자유로운 이미지 크롭 기능 지원
- **[Shimmer-Android](https://github.com/facebook/shimmer-android)**: 데이터가 로드되기 전 스켈레톤(Skeleton) UI를 보여주어 UX 개선
- **[MPAndroidChart](https://github.com/PhilJay/MPAndroidChart)**: 사용자의 독서 통계 데이터(월별 읽은 책 수 등)를 시각화하는 차트 제공
- **[Android-SpinKit](https://github.com/ybq/Android-SpinKit)**: 다양한 로딩 인디케이터(Spinner) 애니메이션 제공
- **[Konfetti](https://github.com/DanielMartinus/Konfetti)**: 챌린지 당성 및 특정 목표 달성 시 화면에 폭죽 애니메이션을 그려주는 파티클 시스템
- **[Navigation Component](https://developer.android.com/guide/navigation)**: 복잡한 Fragment 화면 전환 및 백스택(Backstack)의 흐름 관리
- **[Paging 3](https://developer.android.com/topic/libraries/architecture/paging/v3-overview)**: 대량의 피드나 리스트 데이터를 효율적으로 페이징 처리하여 메모리 부하 방지 및 무한 스크롤(Pulls-to-refresh) 지원
- **[Dexter](https://github.com/Karumi/Dexter)**: 카메라 및 갤러리 접근 등 런타임 권한(Permission) 요청을 간소화하여 처리

<br>

---

본 프로젝트는 안드로이드 생태계의 모던 라이브러리(Jetpack)와 다양한 외부 API 연동을 통해 안정적이고 시각적인 디테일이 돋보이는 모바일 서비스를 제공하는 것을 목표로 합니다.
