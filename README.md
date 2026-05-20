# 이유식 노트

SNS, 블로그, 유튜브 등에서 본 유아식 레시피를 URL·텍스트·이미지로 붙여넣으면 Claude AI가 자동으로 레시피를 추출해 저장해주는 웹앱입니다. 가족과 함께 레시피를 공유할 수 있습니다.

## 파일 구성

```
webapp/
├── index.html              # 앱 구조 (HTML)
├── styles.css              # 커스텀 스타일 (Tailwind는 CDN)
├── app.js                  # 모든 로직 (상태, 렌더링, Firebase, Claude API)
├── firebase-config.js      # Firebase 설정 (git 제외 — 직접 생성 필요)
├── firebase-config.example.js  # Firebase 설정 예시
├── claude-config.js        # Claude API 키 (git 제외 — 직접 생성 필요)
├── claude-config.example.js    # Claude API 키 예시
├── manifest.json           # PWA 설정
├── service-worker.js       # 오프라인 캐시
└── README.md               # 이 문서
```

## 시작하기

### 1. Firebase 설정

`firebase-config.example.js`를 복사해 `firebase-config.js`를 만들고 Firebase 콘솔에서 발급받은 값을 채웁니다.

```js
// firebase-config.js
window.firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};
```

### 2. Claude API 키 설정

`claude-config.example.js`를 복사해 `claude-config.js`를 만들고 API 키를 채웁니다.
API 키는 [console.anthropic.com](https://console.anthropic.com) → API Keys에서 발급받을 수 있습니다.

```js
// claude-config.js
window.claudeConfig = {
  apiKey: "sk-ant-...",
};
```

### 3. 로컬 서버 실행

```bash
# Python
python3 -m http.server 8000

# Node.js
npx serve .
```

브라우저에서 `http://localhost:8000` 접속 후 Google 로그인.

## 기능

### AI 레시피 추출 (신규)
- **URL 입력**: 블로그, 레시피 사이트 URL을 붙여넣으면 내용 자동 추출
- **텍스트 붙여넣기**: 인스타·틱톡 캡션, 유튜브 자막 등 텍스트 직접 붙여넣기
- **이미지 업로드**: 스크린샷 이미지를 올리면 Claude Vision으로 레시피 인식
- 추출 후 편집 폼에 자동으로 채워지며 수정 가능
- 여러 레시피가 추출될 경우 개별 선택 또는 전체 한꺼번에 저장 가능

### 레시피 관리
- 레시피 등록/수정/삭제 (대표 사진, 이름, 조리시간, 분량, 재료, 조리순서, 메모, 태그, 원본 링크)
- 즐겨찾기 토글
- 검색 (이름 / 재료 / 태그)
- 필터 (전체 / 즐겨찾기 / 태그별)
- 재료로 레시피 찾기 (매칭률 %, 부족한 재료 표시)
- JSON 백업 / 복원
- 가족 공유 (로그인한 멤버 전원이 동일한 레시피 풀 공유)

## 데이터 저장

모든 데이터는 **Firebase Firestore**에 저장됩니다. 이미지는 **Firebase Storage**에 저장됩니다.

- 실시간 동기화: 가족 구성원 모두 같은 레시피를 실시간으로 공유
- 오프라인 지원: Firestore 캐시로 오프라인 조회 가능
- 사진은 업로드 시 자동 리사이즈 (대표: 400px, 스텝: 300px)

## 기술 스택

- HTML + CSS + Vanilla JavaScript (빌드 도구 없음)
- Tailwind CSS (CDN)
- Firebase (Authentication · Firestore · Storage)
- Claude API — Haiku 모델 (AI 레시피 추출, 레시피 1개당 약 3~8원)
- FileReader + Canvas (이미지 업로드 + 리사이즈)
- PWA (manifest + service worker)

## AI 추출 소스별 안내

| 소스 | 방법 |
|------|------|
| 블로그, 레시피 사이트 | URL 탭에 링크 붙여넣기 |
| 유튜브 | URL 탭에 링크 붙여넣기 |
| 인스타그램, 틱톡 | 텍스트 탭에 캡션 복사 또는 이미지 탭에 스크린샷 업로드 |

## 확장 방향 (Phase 3 이후)

- 레시피 공개/비공개 설정
- 다른 가족의 공개 레시피 탐색(Explore)
- 레시피 스크랩 / 복사(포크)
- 작성자 페이지
- 신고 기능

## 라이선스

개인 프로젝트용 예시 코드. 자유롭게 수정해서 사용하세요.
