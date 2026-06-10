# Inbody Tracker - AGENT.md

본 문서는 **Inbody Tracker** 모바일 프로그래밍 프로젝트의 AI 개발 에이전트(Antigravity) 작업 내역 및 소스코드 구성 현황을 기록한 명세서입니다.

---

## 📌 프로젝트 개요
*   **프로젝트명**: Inbody Tracker (체성분 변화 기록 및 AI 피드백 건강관리 앱)
*   **원격 저장소**: [GitHub - sonseongyoon/son](https://github.com/sonseongyoon/son)
*   **개발 환경**: Android SDK 35 (API 24+ 호환), Kotlin, Jetpack Compose UI
*   **AI 모델**: `gemini-2.5-flash`

---

## 🛠️ 개발 에이전트 작업 기록

### 1. 컴파일 에러 해결 및 아키텍처 정립
*   **다크모드 설정 동기화**: `MainActivity.kt`에서 테마 스위칭에 필요한 `darkThemeSetting` 상태를 선언하고 설정 저장소(`PreferencesManager`)와 연동하여 버튼 글자 등이 가려지는 UI 버그를 해결했습니다.
*   **이동 콜백 연동**: 측정 기록 저장 성공 시 홈 화면으로 부드럽게 복귀할 수 있도록 `MeasurementScreen`과 `MainActivity` 간의 `onSave` 파라미터 연동 에러를 해결했습니다.
*   **최신 컴포즈 스펙 반영**: Deprecated된 `TextFieldDefaults.outlinedTextFieldColors` 호출을 최신 `OutlinedTextFieldDefaults.colors`로 수정했습니다.

### 2. 제미나이 API 연동 고도화
*   **보안 API 키 주입**: GitHub Push Protection 정책 준수를 위해 API 키값을 소스코드에 하드코딩하지 않고, `local.properties`에 안전하게 정의한 후 `build.gradle.kts`를 통해 `BuildConfig.GEMINI_API_KEY`로 컴파일 시점에 자동 주입되도록 구현했습니다.
*   **1:1 PT 트레이너 페르소나 적용**: AI 챗봇의 기본 지침을 단순한 챗봇이 아닌 친근하고 전문적이며 에너제틱한 **"1:1 전담 PT 선생님"**으로 변경했습니다. 사용자를 "회원님"으로 지칭하고 격려와 전문적인 루틴을 상세하게 제공합니다.
*   **누적 변화량 추세 분석**: 단일 데이터만 전달하던 방식에서 탈피하여 사용자의 최초 측정일 대비 현재 측정 데이터의 변화값(체중/근육량/체지방 증감)을 AI에게 전달하여 유기적인 추세 피드백을 주도하도록 개선했습니다.

### 3. Modern UI 개편
*   **퀵 제안 칩(Quick Suggestion Chips)**: 채팅 화면 상단에 자주 사용하는 헬스 케어 질문 3종(*🏋️‍♂️ 오늘 맞춤 루틴 짜줘!*, *🥗 탄단지 식단 비율 추천*, *📈 인바디 분석 및 목표 팁*)을 배치하여 한 번의 터치로 간편하게 질의할 수 있게 구현했습니다.
*   **마크다운 볼드 렌더러**: 제미나이가 반환한 운동 루틴 명칭 등 강조 텍스트(`**텍스트**`)가 대화 말풍선 내부에서 굵은 글씨로 깔끔하게 처리되도록 커스텀 렌더러를 구축했습니다.
*   **입체감 있는 디자인 톤앤매너**: 분석, 히스토리, 설정 화면 전반의 카드 컴포넌트에 은은한 쉐도우와 둥근 모서리(`RoundedCornerShape(16.dp)`)를 일괄 적용하여 프리미엄 테마를 구축했습니다.

---

## 📂 주요 소스코드 구조 및 역할

| 파일 경로 | 주요 역할 및 기능 설명 |
| :--- | :--- |
| **[MainActivity.kt](file:///Users/sonseong-yun/Downloads/20222356_손성윤/app/src/main/java/com/example/a20222356sddas/MainActivity.kt)** | 앱의 엔트리 포인트, 하단 5개 탭 내비게이션 바 및 화면 라우팅 제어 |
| **[GeminiClient.kt](file:///Users/sonseong-yun/Downloads/20222356_손성윤/app/src/main/java/com/example/a20222356sddas/network/GeminiClient.kt)** | 제미나이 API 통신 처리(`gemini-2.5-flash`), 네트워크 오류 시 모컬 시뮬레이터 자동 폴백 구성 |
| **[ChatScreen.kt](file:///Users/sonseong-yun/Downloads/20222356_손성윤/app/src/main/java/com/example/a20222356sddas/ui/screens/ChatScreen.kt)** | 1:1 AI PT 상담 화면, 퀵 칩 메뉴, 마크다운 텍스트 렌더링, 타이핑 로딩 표시 |
| **[DashboardScreen.kt](file:///Users/sonseong-yun/Downloads/20222356_손성윤/app/src/main/java/com/example/a20222356sddas/ui/screens/DashboardScreen.kt)** | 홈 화면, 2x3 형태의 체성분 핵심 수치 카드 그리드 및 AI 한 줄 인사이트 제공 |
| **[AnalysisScreen.kt](file:///Users/sonseong-yun/Downloads/20222356_손성윤/app/src/main/java/com/example/a20222356sddas/ui/screens/AnalysisScreen.kt)** | 상세 분석 화면, 각 체성분 탭별 추이 선 그래프 시각화 및 최저/최고/평균 통계 카드 |
| **[GoalTodoScreen.kt](file:///Users/sonseong-yun/Downloads/20222356_손성윤/app/src/main/java/com/example/a20222356sddas/ui/screens/GoalTodoScreen.kt)** | 건강 목표 설정, AI 기반 주간 감량 한계선 경고 검증 및 AI 맞춤 일일 투두 리스트 생성 |
| **[HistoryScreen.kt](file:///Users/sonseong-yun/Downloads/20222356_손성윤/app/src/main/java/com/example/a20222356sddas/ui/screens/HistoryScreen.kt)** | 전체 측정 이력 리스트, 상대적 시간 경과 배지("오늘", "1주 전" 등) 및 기록 개별 삭제 기능 |
| **[MeasurementScreen.kt](file:///Users/sonseong-yun/Downloads/20222356_손성yun/app/src/main/java/com/example/a20222356sddas/ui/screens/MeasurementScreen.kt)** | 새로운 체성분(체중, 골격근량, 체지방률, 체수분 등) 데이터 등록 입력 폼 |
| **[SettingsScreen.kt](file:///Users/sonseong-yun/Downloads/20222356_손성윤/app/src/main/java/com/example/a20222356sddas/ui/screens/SettingsScreen.kt)** | 사용자 프로필 수정, 화면 다크 모드 활성화/비활성화, PIN 보안 잠금 번호 등록 |
| **[PreferencesManager.kt](file:///Users/sonseong-yun/Downloads/20222356_손성윤/app/src/main/java/com/example/a20222356sddas/data/PreferencesManager.kt)** | SharedPreferences 기반 로컬 데이터(인바디 이력, 대화 기록, 설정 값) 직렬화 및 저장소 관리 |
