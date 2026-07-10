# 교내 축구대회

Fotmob 스타일의 학교 축구대회 경기 정보 앱. 순위표, 경기 상세(라인업/타임라인), 선수 평점·MOTM 투표, 관리자 모드(팀/선수/조/경기 등록, 결과·이벤트 입력)를 제공합니다.

서버 없이 정적 페이지 하나(`index.html`)로 동작하며, 데이터는 Firebase Firestore(무료 Spark 요금제)에 저장되어 모든 사용자가 같은 데이터를 실시간으로 봅니다.

## 1. Firebase 설정 (한 번만, 약 5분)

1. [console.firebase.google.com](https://console.firebase.google.com/) 에서 새 프로젝트 생성
2. 왼쪽 메뉴 **Firestore Database** → **데이터베이스 만들기** (테스트 모드로 시작)
3. 프로젝트 설정(톱니바퀴) → **내 앱**에서 웹 앱(`</>`) 추가 → `firebaseConfig` 값 복사
4. `index.html` 상단의 `firebaseConfig` 객체에 그대로 붙여넣기
5. Firestore **규칙** 탭에 아래 규칙을 붙여넣고 게시 (로그인 없이 모두가 읽고 쓸 수 있도록 허용):

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /soccer/{docId} {
      allow read, write: if true;
    }
  }
}
```

## 2. GitHub Pages로 배포

```bash
git add index.html
git commit -m "Update firebaseConfig"
git push
```

그 다음 GitHub 저장소의 **Settings → Pages**에서:
- Source: `Deploy from a branch`
- Branch: `main` / `/ (root)`

저장하면 몇 분 안에 `https://<사용자이름>.github.io/<저장소이름>/` 에서 접속할 수 있습니다.

## 참고

- 관리자 모드는 별도 로그인이 없습니다. 링크(또는 관리자 화면 접근)를 신뢰할 수 있는 사람과만 공유하세요.
- Firestore 보안 규칙이 `allow read, write: if true`이므로 URL과 프로젝트 설정을 아는 사람은 누구나 데이터를 쓸 수 있습니다. 학교 행사용으로는 충분하지만, 더 엄격하게 하려면 규칙에 조건을 추가하세요.
