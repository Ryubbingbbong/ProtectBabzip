# 마을 방어 레이드 스케줄 관리 시스템

실시간 공유 기능이 있는 레이드 스케줄 투표 사이트입니다.

## 주요 기능

- ✅ 실시간 데이터 동기화 (Firebase)
- ✅ 무제한 시간대 투표 가능
- ✅ 10명 이상 시 "추천" 배지 표시
- ✅ 15명 이상 시 자동 우선순위 계산 및 10명 확정
- ✅ 우선순위 시스템:
  - 1순위: 해당 시간만 투표 + 레이드 0번
  - 2순위: 레이드 0번
  - 3순위: 해당 시간만 투표 + 레이드 1번
  - 4순위: 레이드 1번
- ✅ 확정/대기 멤버 구분 표시
- ✅ 주말 4개 + 평일 2개 레이드
- ✅ 평일은 요일 선택 가능
- ✅ 전체 마을원 현황 (투표/확정 횟수)
- ✅ 관리자 비밀번호 보호
- ✅ 매주 수요일 자동 초기화
- ✅ 테스트 데이터 생성 기능
- ✅ 모바일 반응형

## Firebase 설정 방법

### 1. Firebase 프로젝트 생성

1. [Firebase Console](https://console.firebase.google.com/)에 접속
2. "프로젝트 추가" 클릭
3. 프로젝트 이름 입력 (예: `raid-schedule`)
4. Google Analytics는 선택 사항 (필요시 활성화)
5. 프로젝트 생성 완료

### 2. Realtime Database 설정

1. 왼쪽 메뉴에서 **빌드 > Realtime Database** 클릭
2. "데이터베이스 만들기" 클릭
3. 위치 선택 (예: `asia-southeast1`)
4. **테스트 모드**로 시작 선택
   ```
   {
     "rules": {
       ".read": true,
       ".write": true
     }
   }
   ```
   > ⚠️ 테스트 모드는 보안이 약합니다. 실제 운영시에는 보안 규칙을 설정하세요!

5. "사용 설정" 클릭

### 3. 웹 앱 추가

1. 프로젝트 개요 페이지에서 **웹 앱 추가** (</> 아이콘) 클릭
2. 앱 닉네임 입력 (예: `raid-web`)
3. Firebase Hosting은 선택 사항 (나중에 설정 가능)
4. "앱 등록" 클릭

### 4. Firebase 설정 정보 복사

Firebase SDK 구성 정보가 표시됩니다:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "your-project.firebaseapp.com",
  databaseURL: "https://your-project-default-rtdb.firebaseio.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
};
```

### 5. index.html 파일 수정

`index.html` 파일을 열고 다음 부분을 찾아 본인의 Firebase 설정으로 교체하세요:

```javascript
// ⚠️ Firebase 설정 - 여기에 본인의 Firebase 설정을 넣으세요
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",  // 여기를 수정
    authDomain: "your-project.firebaseapp.com",  // 여기를 수정
    databaseURL: "https://your-project.firebaseio.com",  // 여기를 수정
    projectId: "your-project",  // 여기를 수정
    storageBucket: "your-project.appspot.com",  // 여기를 수정
    messagingSenderId: "123456789",  // 여기를 수정
    appId: "your-app-id"  // 여기를 수정
};
```

### 6. 관리자 비밀번호 변경

보안을 위해 관리자 비밀번호를 변경하세요:

```javascript
const ADMIN_PASSWORD = "admin1234"; // ⚠️ 이 비밀번호를 변경하세요!
```

## GitHub Pages 배포 방법

### 1. GitHub 저장소 생성

1. GitHub에 로그인
2. "New repository" 클릭
3. 저장소 이름 입력 (예: `raid-schedule`)
4. Public으로 설정
5. "Create repository" 클릭

### 2. 파일 업로드

1. 생성된 저장소에서 "uploading an existing file" 클릭
2. `index.html` 파일을 드래그 앤 드롭
3. "Commit changes" 클릭

### 3. GitHub Pages 활성화

1. 저장소의 **Settings** 탭 클릭
2. 왼쪽 메뉴에서 **Pages** 클릭
3. **Source**에서 `main` 브랜치 선택
4. **Save** 클릭
5. 몇 분 후 사이트 주소가 표시됩니다
   - 예: `https://[본인계정].github.io/raid-schedule`

## 사용 방법

### 첫 방문 시

1. 사이트 접속
2. "내 정보" 섹션에서 이름 입력 후 "등록" 클릭
3. 원하는 시간대를 클릭하여 투표 (여러 시간대 선택 가능)

### 투표하기

- **시간대 클릭**: 참가 신청
- **다시 클릭**: 참가 취소
- **흰색 테두리**: 내가 선택한 시간대
- **노란색 "추천" 배지**: 10명 이상 투표한 시간대
- **녹색 "확정"**: 15명 이상 투표 시 우선순위에 따라 확정된 멤버
- **주황색 "대기"**: 15명 이상 투표 시 대기 중인 멤버

### 우선순위 시스템 (15명 이상 시 자동 적용)

투표자가 15명 이상일 때 자동으로 10명을 확정합니다:

1. **1순위**: 해당 시간대만 투표 + 아직 확정된 레이드 0번
   - 예: 토요일 1차 18:00만 투표하고 다른 레이드 참가 안 함
   
2. **2순위**: 아직 확정된 레이드 0번
   - 예: 여러 시간대에 투표했지만 아직 확정된 레이드가 없음
   
3. **3순위**: 해당 시간대만 투표 + 확정된 레이드 1번
   - 예: 토요일 1차 18:00만 투표, 일요일 1차는 이미 확정
   
4. **4순위**: 확정된 레이드 1번
   - 예: 여러 시간대 투표, 다른 레이드 1번 확정

### 평일 레이드

- 요일 선택 후 시간대 투표
- 모든 마을원이 같은 요일을 보게 됩니다

### 관리자 기능

- **전체 스케줄 초기화**: 관리자 비밀번호 필요
- **테스트 데이터 추가**: 기능 테스트용 (20명의 가상 투표 생성)
- 매주 수요일 00:00에 자동으로 초기화됩니다

### 테스트 방법

1. "테스트 데이터 추가" 버튼 클릭
2. 토요일 1차 18:00에 다양한 우선순위의 20명 생성
3. 확정/대기 시스템이 정상 작동하는지 확인
4. 테스트 완료 후 "전체 스케줄 초기화"로 데이터 삭제

## 보안 권장사항

### Firebase 보안 규칙 (실제 운영 시)

테스트 모드 대신 다음 규칙 사용을 권장합니다:

```json
{
  "rules": {
    "raidSchedule": {
      ".read": true,
      ".write": true
    }
  }
}
```

더 강력한 보안이 필요하다면:

```json
{
  "rules": {
    "raidSchedule": {
      ".read": true,
      ".write": "auth != null"  // 인증된 사용자만 쓰기 가능
    }
  }
}
```

### 도메인 제한

Firebase Console > 프로젝트 설정 > 승인된 도메인에서:
- GitHub Pages 도메인만 허용
- 예: `your-username.github.io`

## 문제 해결

### "Firebase 설정이 필요합니다" 메시지

- Firebase 설정 정보가 올바르게 입력되었는지 확인
- 브라우저 콘솔(F12)에서 에러 메시지 확인

### "연결 끊김" 상태

- 인터넷 연결 확인
- Firebase Realtime Database가 활성화되어 있는지 확인
- Firebase Console에서 데이터베이스 URL 확인

### 데이터가 저장되지 않음

- Firebase 보안 규칙에서 쓰기 권한 확인
- 브라우저 콘솔에서 에러 메시지 확인

## 라이선스

MIT License

## 기여

버그 리포트나 기능 제안은 이슈로 등록해주세요!
