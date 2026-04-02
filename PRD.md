# Note Quiz — Product Requirements Document

## 1. 개요

| 항목 | 내용 |
|------|------|
| 서비스명 | Note Quiz |
| 버전 | v1.0 |
| 작성일 | 2026-04-01 |
| 한 줄 설명 | 공부 노트(이미지, PDF)를 업로드하면 AI가 객관식 문제를 자동 생성해주는 웹 서비스 |

---

## 2. 문제 정의

### 배경
학습 후 복습 단계에서 문제를 직접 만드는 것은 시간이 많이 걸리고 비효율적이다. 특히 손으로 작성한 노트나 PDF 자료를 기반으로 문제를 제작하려면 내용을 다시 읽고 핵심 개념을 파악한 뒤 문제와 보기를 작성해야 한다.

### 해결하려는 문제
- 노트 내용으로 문제를 수동으로 만드는 시간 낭비
- 복습 효율 저하 (문제 제작에 집중하다 학습 목적 희석)
- 생성한 문제를 재활용하거나 꾸준히 복습할 수단 부재

### 목표
파일 업로드 한 번으로 즉시 복습용 객관식 문제를 받아볼 수 있게 한다. 로그인 사용자에게는 문제 저장, 오답 관리, 알림 등 지속적인 학습 지원 기능을 추가 제공한다.

---

## 3. 타겟 사용자

- 시험 준비 중인 학생 (대학생, 수험생)
- 자격증 공부를 하는 직장인
- 강의 자료를 복습하고 싶은 학습자

---

## 4. 기능 등급 (Feature Tier)

모든 핵심 기능은 비로그인 상태에서도 사용 가능하다. 로그인 시 학습 이력 관리 및 알림 기반의 추가 기능이 활성화된다.

| 기능 | 비로그인 | 로그인 |
|------|:--------:|:------:|
| 파일 업로드 (PDF, 이미지) | O | O |
| 문제 생성 (AI) | O | O |
| 퀴즈 풀기 | O | O |
| 결과 확인 | O | O |
| 생성한 문제 저장 및 열람 | X | O |
| 문제 공유 | X | O |
| 오답 모아보기 | X | O |
| 공부 알림 (푸시/이메일) | X | O |
| 매일 자동 문제 생성 알림 | X | O |

---

## 5. 핵심 기능 명세

### F1. 파일 업로드 `(공통)`
- 지원 형식: PDF, PNG, JPG, JPEG
- 파일 크기 제한: 20MB 이하
- UI: 드래그 앤 드롭 및 클릭하여 파일 선택
- 업로드 후 파일명과 미리보기(썸네일) 표시

### F2. 문제 생성 설정 `(공통)`
- 생성할 문제 수 선택: 5개 / 10개 / 20개
- "문제 생성" 버튼 클릭으로 생성 시작
- 생성 중 로딩 스피너 및 진행 상태 메시지 표시

### F3. 문제 생성 (AI) `(공통)`
- 업로드 파일에서 텍스트 추출 (PDF → PDFBox, 이미지 → Tesseract OCR)
- 추출된 텍스트를 로컬 LLM(Ollama)에 전달하여 문제 생성
- 각 문제 구성: 문제 본문, 보기 4개, 정답 번호, 해설

### F4. 퀴즈 풀기 `(공통)`
- 문제를 한 개씩 순서대로 표시
- 보기 4개 중 하나 선택
- 선택 즉시 정답/오답 여부 피드백 표시
- 정답 및 해설 표시
- 다음 문제로 이동

### F5. 결과 확인 `(공통)`
- 전체 문제 수 대비 정답 수 및 정답률(%) 표시
- 오답 문제 목록과 해설 재확인
- "다시 풀기" 및 "새 파일 업로드" 버튼 제공
- 로그인 사용자: 결과 자동 저장 및 "오답 노트에 추가" 버튼 표시

### F6. 문제 저장 및 열람 `(로그인 전용)`
- 문제 생성 완료 시점에 자동으로 내 퀴즈 목록에 저장
- 마이페이지에서 생성 일시, 파일명, 문제 수와 함께 목록 조회
- 저장된 퀴즈 다시 풀기 가능

### F7. 문제 공유 `(로그인 전용)`
- **내 퀴즈 목록**에서만 공유 가능 (결과 화면에서는 제공하지 않음)
- 저장된 퀴즈의 문제 자체(문제 본문, 보기, 정답, 해설)를 고유 링크로 공유
- 링크 접속 시 비로그인 포함 누구나 해당 퀴즈를 풀 수 있음
- 공유 링크 복사 버튼 제공

### F8. 오답 모아보기 `(로그인 전용)`
- 틀린 문제만 모아 별도 오답 노트 페이지에서 확인
- 오답 문제를 다시 풀 수 있는 "오답 퀴즈 시작" 기능
- 오답 해결 시 오답 노트에서 제거 (선택)

### F9. 공부 알림 `(로그인 전용)`
- 사용자가 알림 시간대를 직접 설정 (예: 매일 오후 8시)
- 설정한 시간에 "오늘 복습할 시간이에요!" 푸시 또는 이메일 알림 발송
- 알림 켜기/끄기 토글 지원

### F10. 매일 자동 문제 생성 알림 `(로그인 전용)`
- 사용자가 저장한 노트(파일) 중 하나를 매일 자동으로 선택하여 문제 생성
- 생성된 문제를 알림과 함께 발송 ("오늘의 퀴즈가 생성되었어요!")
- 알림 클릭 시 해당 퀴즈 바로 이동

---

## 6. 인증 (Authentication)

- 이메일 + 비밀번호 기반 회원가입 / 로그인
- 소셜 로그인 (Google OAuth) — 선택적 구현
- 인증 방식: JWT (Access Token + Refresh Token)
- 비로그인 세션: 브라우저 `sessionStorage`로 임시 퀴즈 데이터 관리 (탭 닫으면 소멸)
- 로그인 세션: 서버 DB에 퀴즈 데이터 영구 저장

---

## 7. 사용자 흐름 (User Flow)

### 비로그인 흐름

```
[홈 화면]
  └─ 파일 업로드 (PDF / 이미지)
       └─ 문제 수 선택 (5 / 10 / 20)
            └─ "문제 생성" 클릭
                 └─ [로딩] AI 분석 중...
                      └─ [퀴즈 화면] 문제 풀기
                           └─ [결과 화면] 점수 및 오답 확인
                                ├─ 다시 풀기
                                └─ 새 파일 업로드
```

### 로그인 흐름

```
[홈 화면] (헤더에 로그인 상태 표시)
  ├─ 파일 업로드 (PDF / 이미지)
  │    └─ 문제 수 선택 (5 / 10 / 20)
  │         └─ "문제 생성" 클릭
  │              └─ [로딩] AI 분석 중...
  │                   └─ 내 퀴즈 목록에 자동 저장
  │                        └─ [퀴즈 화면] 문제 풀기
  │                             └─ [결과 화면]
  │                                  ├─ 결과 자동 저장
  │                                  ├─ 오답 노트에 추가
  │                                  ├─ 다시 풀기
  │                                  └─ 새 파일 업로드
  │
  └─ [마이페이지 - 내 퀴즈 목록]
       └─ 저장된 퀴즈 선택
            ├─ 풀기
            ├─ 공유 링크 생성 → 링크 복사
            └─ 삭제
```

### 공유 링크 접속 흐름

```
[공유 URL 접속] (비로그인 포함 누구나 가능)
  └─ 공유된 퀴즈 문제 풀기
       └─ 결과 확인
```

---

## 8. 기술 아키텍처

### 전체 구성

```
[브라우저]
  React (Vite)
      │  HTTP (REST API + JWT)
      ▼
[Spring Boot 서버]
  - 파일 수신 및 텍스트 추출
  - 인증/인가 (JWT)
  - LLM 연동 (Ollama API 호출)
  - 알림 스케줄러 (Spring Scheduler)
  - 문제/사용자 데이터 DB 저장
      │              │  HTTP
      ▼              ▼
[RDB - H2]      [Ollama 서버 - 로컬]
  - 사용자          - llama3 / mistral
  - 퀴즈            - POST /api/generate
  - 오답 기록
  - 알림 설정
```

### Frontend
| 항목 | 내용 |
|------|------|
| 프레임워크 | React 18 |
| 빌드 도구 | Vite |
| 라우팅 | React Router v6 |
| HTTP 클라이언트 | Axios (인터셉터로 JWT 자동 첨부) |
| 스타일링 | CSS Modules 또는 Tailwind CSS |

**주요 페이지**
- `/` — 홈 (파일 업로드 + 문제 수 선택)
- `/quiz/:quizId` — 퀴즈 풀기
- `/result/:quizId` — 결과 확인
- `/login` — 로그인
- `/signup` — 회원가입
- `/my` — 마이페이지 (로그인 전용)
- `/my/quizzes` — 내 퀴즈 목록 (로그인 전용)
- `/my/wrong` — 오답 노트 (로그인 전용)
- `/my/settings` — 알림 설정 (로그인 전용)
- `/share/:quizId` — 공유 퀴즈

### Backend
| 항목 | 내용 |
|------|------|
| 프레임워크 | Spring Boot 3.x |
| 언어 | Java 17+ |
| 인증 | Spring Security + JWT |
| PDF 파싱 | Apache PDFBox |
| 이미지 OCR | Tesseract (tess4j) |
| LLM 클라이언트 | Spring WebClient |
| 스케줄러 | Spring Scheduler (알림/자동 문제 생성) |
| 데이터베이스 | H2 + Spring Data JPA |
| 빌드 도구 | Gradle 또는 Maven |

### 로컬 LLM
| 항목 | 내용 |
|------|------|
| 서버 | Ollama |
| 모델 | llama3 또는 mistral (설정으로 변경 가능) |
| 호출 방식 | REST API — `POST http://localhost:11434/api/generate` |

---

## 9. DB 스키마 (주요 테이블)

```
users
  - id, email, password_hash, nickname, created_at

uploads
  - id, upload_id (UUID), extracted_text (TEXT), created_at

quizzes
  - id, user_id (nullable), upload_id, title, source_filename, created_at, share_token

questions
  - id, quiz_id, body, options (JSON), answer, explanation, order_num

quiz_results
  - id, quiz_id, user_id (nullable), score, total, created_at

wrong_answers
  - id, user_id, question_id, resolved, created_at

notification_settings
  - id, user_id, study_reminder_enabled, reminder_time,
    daily_quiz_enabled, daily_quiz_time
```

---

## 10. API 설계

### 인증

| Method | Endpoint | 인증 필요 | 설명 |
|--------|----------|:---------:|------|
| POST | `/api/auth/signup` | X | 회원가입 |
| POST | `/api/auth/login` | X | 로그인, JWT 반환 |
| POST | `/api/auth/refresh` | X | Access Token 갱신 |

### 파일 및 문제 생성

| Method | Endpoint | 인증 필요 | 설명 |
|--------|----------|:---------:|------|
| POST | `/api/upload` | X | 파일 업로드 및 텍스트 추출 |
| POST | `/api/quiz/generate` | X | 문제 생성 (비로그인 가능, 로그인 시 자동 저장) |
| GET | `/api/quiz/{quizId}` | X | 퀴즈 조회 |
| POST | `/api/quiz/{quizId}/result` | X | 퀴즈 결과 제출 |

### 로그인 전용

| Method | Endpoint | 인증 필요 | 설명 |
|--------|----------|:---------:|------|
| GET | `/api/my/quizzes` | O | 내 퀴즈 목록 조회 |
| DELETE | `/api/my/quizzes/{quizId}` | O | 내 퀴즈 삭제 |
| GET | `/api/my/wrong` | O | 오답 목록 조회 |
| PATCH | `/api/my/wrong/{wrongId}/resolve` | O | 오답 해결 처리 |
| POST | `/api/my/quizzes/{quizId}/share` | O | 내 퀴즈 공유 링크 생성 |
| GET | `/api/share/{shareToken}` | X | 공유 퀴즈 조회 |
| GET | `/api/my/settings/notification` | O | 알림 설정 조회 |
| PUT | `/api/my/settings/notification` | O | 알림 설정 변경 |

---

## 11. 화면 설계 (Wireframe 개요)

### 홈 화면
```
┌─────────────────────────────────────┐
│  Note Quiz          [로그인] [가입] │  ← 로그인 시: [내 퀴즈] [jeong ▼]
│                                     │
│   ┌─────────────────────────────┐   │
│   │                             │   │
│   │   파일을 드래그하거나         │   │
│   │   클릭하여 업로드하세요      │   │
│   │   (PDF, PNG, JPG)           │   │
│   └─────────────────────────────┘   │
│                                     │
│   문제 수:  [5]  [10]  [20]         │
│                                     │
│        [  문제 생성하기  ]          │
└─────────────────────────────────────┘
```

### 퀴즈 화면
```
┌─────────────────────────────────┐
│  3 / 10                         │
│  ━━━━━━━━━━░░░░░░░░░░░░ 30%    │
│                                 │
│  Q. 문제 본문이 여기 표시됩니다. │
│                                 │
│  ① 보기 1                      │
│  ② 보기 2  ◀ 선택 시 하이라이트│
│  ③ 보기 3                      │
│  ④ 보기 4                      │
│                                 │
│  [정답입니다! 해설: ...]         │
│                     [다음 문제] │
└─────────────────────────────────┘
```

### 결과 화면
```
┌──────────────────────────────────────┐
│              결과                    │
│                                      │
│            8 / 10  (80%)             │
│                                      │
│  오답 문제                            │
│  ├ Q3. 문제...  (정답: ②)            │
│  └ Q7. 문제...  (정답: ④)            │
│                                      │
│  [다시 풀기]  [새 파일 업로드]         │
│  [오답 노트에 추가]  ← 로그인 시만 표시│
└──────────────────────────────────────┘
```

### 마이페이지 — 내 퀴즈 목록
```
┌─────────────────────────────────────┐
│  내 퀴즈                            │
│                                     │
│  ┌───────────────────────────────┐  │
│  │ 운영체제 정리.pdf              │  │
│  │ 10문제  ·  2026-04-01         │  │
│  │ [풀기]  [공유]  [삭제]        │  │
│  └───────────────────────────────┘  │
│  ┌───────────────────────────────┐  │
│  │ 데이터베이스 노트.png          │  │
│  │ 5문제  ·  2026-03-30          │  │
│  │ [풀기]  [공유]  [삭제]        │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
```

### 마이페이지 — 알림 설정
```
┌─────────────────────────────────────┐
│  알림 설정                           │
│                                     │
│  공부 리마인더                        │
│  매일 정해진 시간에 알림을 보냅니다.  │
│  [ON / OFF]  시간: [오후 08:00 ▼]   │
│                                     │
│  오늘의 퀴즈                          │
│  저장된 노트로 매일 문제를 생성합니다. │
│  [ON / OFF]  시간: [오전 09:00 ▼]   │
│                                     │
│              [저장]                  │
└─────────────────────────────────────┘
```

---

## 12. 비기능 요구사항

| 항목 | 기준 |
|------|------|
| 응답 시간 | 파일 업로드 후 문제 생성 완료까지 30초 이내 (LLM 성능에 따라 변동) |
| 파일 처리 | 업로드 파일은 텍스트 추출 후 서버에서 삭제 (개인정보 보호) |
| 인증 보안 | JWT Access Token 만료: 1시간, Refresh Token 만료: 30일 |
| 비밀번호 | bcrypt 해시 저장 |
| 동시 사용자 | 초기: 소규모 다중 사용자 기준으로 구조 설계 |
| 브라우저 지원 | Chrome, Firefox, Safari 최신 버전 |

---

## 13. 범위 외 (Out of Scope — v1.0)

- 소셜 로그인 (Google OAuth) — 선택적 구현, 우선순위 낮음
- 모바일 앱 (iOS / Android)
- 단답형, OX, 서술형 등 다른 문제 유형
- 클라우드 LLM 연동 (OpenAI, Claude API 등)
- 팀/그룹 스터디 기능

---

## 14. 향후 고려 사항 (Future Considerations)

- 문제 유형 확장 (단답형, OX, 빈칸 채우기)
- 여러 파일 동시 업로드 및 통합 문제 생성
- 문제 난이도 조절 옵션
- 학습 통계 및 성장 그래프
- 팀/그룹 스터디 공유 기능

---

## 15. API 상세 스펙

### 에러 응답 공통 형식

```json
{
  "code": "QUIZ_NOT_FOUND",
  "message": "퀴즈를 찾을 수 없습니다.",
  "status": 404
}
```

### 에러 코드 정의

| 코드 | HTTP 상태 | 설명 |
|------|-----------|------|
| `INVALID_FILE_FORMAT` | 400 | 지원하지 않는 파일 형식 |
| `FILE_TOO_LARGE` | 400 | 파일 크기 초과 (20MB) |
| `TEXT_EXTRACT_FAILED` | 422 | 텍스트 추출 실패 |
| `LLM_TIMEOUT` | 504 | LLM 응답 시간 초과 |
| `QUIZ_NOT_FOUND` | 404 | 퀴즈 없음 |
| `UNAUTHORIZED` | 401 | 인증 필요 |
| `FORBIDDEN` | 403 | 접근 권한 없음 |
| `TOKEN_EXPIRED` | 401 | 액세스 토큰 만료 |

### POST /api/auth/signup

Request:
```json
{
  "email": "user@example.com",
  "password": "Password1!",
  "nickname": "jeong"
}
```

Response `201`:
```json
{
  "userId": 1,
  "email": "user@example.com",
  "nickname": "jeong"
}
```

### POST /api/auth/login

Request:
```json
{
  "email": "user@example.com",
  "password": "Password1!"
}
```

Response `200`:
```json
{
  "accessToken": "eyJ...",
  "expiresIn": 3600
}
```

- Refresh Token은 `HttpOnly` 쿠키로 자동 설정

### POST /api/auth/refresh

- Refresh Token은 쿠키에서 자동 추출

Response `200`:
```json
{
  "accessToken": "eyJ...",
  "expiresIn": 3600
}
```

### POST /api/upload

- Content-Type: `multipart/form-data`
- Field: `file`

Response `200`:
```json
{
  "uploadId": "abc-123",
  "filename": "운영체제_정리.pdf",
  "extractedTextLength": 3420
}
```

### POST /api/quiz/generate

Request:
```json
{
  "uploadId": "abc-123",
  "questionCount": 10
}
```

Response `201`:
```json
{
  "quizId": 42,
  "questions": [
    {
      "id": 1,
      "body": "다음 중 프로세스 스케줄링 알고리즘이 아닌 것은?",
      "options": ["FCFS", "Round Robin", "LRU", "SJF"],
      "answer": 3,
      "explanation": "LRU는 페이지 교체 알고리즘입니다."
    }
  ]
}
```

### POST /api/quiz/{quizId}/result

Request:
```json
{
  "answers": [
    { "questionId": 1, "selected": 3 },
    { "questionId": 2, "selected": 1 }
  ]
}
```

Response `200`:
```json
{
  "resultId": 99,
  "score": 8,
  "total": 10,
  "wrongQuestions": [
    { "questionId": 3, "selected": 2, "answer": 4 },
    { "questionId": 7, "selected": 1, "answer": 3 }
  ]
}
```

### POST /api/my/quizzes/{quizId}/share

Response `200`:
```json
{
  "shareToken": "xK92mP",
  "shareUrl": "/share/xK92mP"
}
```

---

## 16. 파일 처리 파이프라인

```
[클라이언트]
  └─ multipart/form-data 업로드
       └─ [Spring Boot] 파일 수신
            ├─ 확장자 검증 (PDF, PNG, JPG, JPEG)
            ├─ 크기 검증 (≤ 20MB)
            └─ 텍스트 추출
                 ├─ PDF  → Apache PDFBox → 페이지별 텍스트 합산
                 └─ 이미지 → Tesseract OCR (tess4j, 한국어 + 영어)
                      └─ 텍스트 정제 (연속 공백·특수문자 정규화)
                           └─ uploadId 및 추출 텍스트를 DB에 저장
                                └─ 업로드 완료 응답 반환
```

- 추출된 텍스트는 `uploads` 테이블에 `uploadId`와 함께 저장
- OCR 언어팩: `kor + eng` 동시 인식

---

## 17. LLM 연동 상세

### Ollama 호출 방식

- Endpoint: `POST http://localhost:11434/api/generate`
- 타임아웃: 30초
- 실패 시 1회 재시도 후 `LLM_TIMEOUT` 에러 반환

### 프롬프트 템플릿

```
다음 텍스트를 바탕으로 객관식 문제 {questionCount}개를 만들어줘.

규칙:
1. 각 문제는 4개의 보기를 가진다.
2. 정답은 1~4 중 하나의 번호로 표시한다.
3. 반드시 아래 JSON 형식으로만 응답한다. 다른 텍스트는 포함하지 않는다.

텍스트:
{extractedText}

응답 형식:
[
  {
    "body": "문제 본문",
    "options": ["보기1", "보기2", "보기3", "보기4"],
    "answer": 1,
    "explanation": "해설"
  }
]
```

### 응답 파싱

```
LLM 응답 문자열
  └─ 정규식으로 JSON 배열 추출 (\[[\s\S]*\])
       ├─ 파싱 성공 → questions 테이블에 저장
       └─ 파싱 실패 → TEXT_EXTRACT_FAILED 에러 반환
```

---

## 18. 인증 흐름 상세

### JWT 구성

| 항목 | 값 |
|------|----|
| Access Token 만료 | 1시간 |
| Refresh Token 만료 | 30일 |
| 서명 알고리즘 | HS256 |
| Access Token 전달 | `Authorization: Bearer {token}` 헤더 |
| Refresh Token 전달 | `HttpOnly` 쿠키 |

### 토큰 갱신 시퀀스

```
[클라이언트]                         [서버]
     │  API 요청 (만료된 토큰)         │
     │ ──────────────────────────────► │
     │  401 TOKEN_EXPIRED              │
     │ ◄────────────────────────────── │
     │                                 │
     │  POST /api/auth/refresh         │
     │  (쿠키의 Refresh Token 자동 전송)│
     │ ──────────────────────────────► │
     │  새 Access Token 발급           │
     │ ◄────────────────────────────── │
     │                                 │
     │  원래 API 재요청                 │
     │ ──────────────────────────────► │
```

- Axios 인터셉터에서 401 감지 → 자동 토큰 갱신 → 원래 요청 재시도
- Refresh Token도 만료된 경우 → 로그인 페이지로 리다이렉트

---

## 19. 비로그인 세션 & 공유 토큰

### 비로그인 세션

- 퀴즈 데이터를 `sessionStorage['nq_quiz_{quizId}']` 키로 저장
- 탭을 닫으면 자동 소멸
- 구조:

```json
{
  "quizId": "temp-uuid",
  "questions": [...],
  "createdAt": "2026-04-02T10:00:00Z"
}
```

### 공유 토큰

- 생성: `UUID.randomUUID()` 앞 6자리 (예: `xK92mP`)
- 충돌 시 재생성 (최대 3회)
- 만료 없음 — 퀴즈 삭제 시 함께 삭제
- 공유 URL: `/share/{shareToken}`

---

## 20. 알림 스케줄러

| 알림 종류 | 발송 수단 | 처리 방식 |
|-----------|-----------|-----------|
| 공부 리마인더 | 이메일 (JavaMailSender) | 매분 실행, `reminder_time` 일치 사용자 조회 후 발송 |
| 오늘의 퀴즈 | 이메일 | 매분 실행, `daily_quiz_time` 일치 사용자 조회 후 문제 생성 및 발송 |

```java
@Scheduled(cron = "0 * * * * *")  // 매 분 0초마다 실행
public void sendStudyReminders() { ... }
```

- 오늘의 퀴즈: 사용자 저장 파일 중 `RANDOM()` 1개 선택 → LLM 문제 생성 → 이메일 발송
- 발송 실패 시 로그만 기록, 재시도 없음 (v1.0)

---

## 21. 에러 처리

### 백엔드

- `@RestControllerAdvice`로 전역 예외 핸들러 구성
- 모든 에러는 15절의 공통 응답 형식으로 반환

### 프론트엔드

| 상황 | UX |
|------|----|
| 파일 형식 / 크기 오류 | 업로드 영역 아래 인라인 에러 메시지 |
| 문제 생성 실패 | 모달 알림 + "다시 시도" 버튼 |
| 네트워크 오류 | 토스트 메시지 |
| 인증 만료 | Axios 인터셉터 자동 갱신 → 실패 시 로그인 페이지 이동 |

---

## 22. 환경 구성

### 환경 변수

**Backend** (`application.yml` 또는 `.env`)
```
DB_URL=jdbc:h2:file:./data/notequiz
DB_USERNAME=sa
DB_PASSWORD=

JWT_SECRET=your-256-bit-secret
JWT_ACCESS_EXPIRY=3600
JWT_REFRESH_EXPIRY=2592000

OLLAMA_BASE_URL=http://localhost:11434
OLLAMA_MODEL=llama3
OLLAMA_TIMEOUT=30

MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=your@gmail.com
MAIL_PASSWORD=app-password
```

**Frontend** (`.env`)
```
VITE_API_BASE_URL=http://localhost:8080
```

### 로컬 개발 환경 구성 순서

```
1. H2 스키마 자동 생성 (애플리케이션 시작 시 자동 처리)
2. Ollama 설치 및 모델 pull
     └─ ollama pull llama3
3. Backend 실행
     └─ 환경변수 주입 후 ./gradlew bootRun
4. Frontend 실행
     └─ npm install && npm run dev
```
