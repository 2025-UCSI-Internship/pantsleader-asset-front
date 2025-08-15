# 2025 UCSI Malaysia Internship - Frontend

본 레포지토리는 **2025 UCSI 말레이시아 인턴십**에서 진행한 **Frontend 프로젝트**를 관리합니다.  
SvelteKit 기반으로 구현되었으며, 자산 관리 시스템(Asset Management System)의 UI를 담당합니다.

---

## 📌 주요 기능
- 로그인 / 회원가입 페이지
- 자산 목록 조회
- 자산 상세 조회
- 자산 등록 / 수정
- 자산 이동 기록(대출 / 반납)
- 반응형 UI 지원

---

## 🛠 기술 스택
- **Framework**: [SvelteKit](https://kit.svelte.dev/)
- **Language**: JavaScript
- **Styling**: Tailwind CSS
- **Build Tool**: Vite
- **Version Control**: Git / GitHub

---

## 📂 폴더 구조
internproject2/
├── src/ # 메인 소스 코드
│ ├── lib/ # 컴포넌트, 스토어 등 공용 라이브러리
│ ├── routes/ # 페이지 라우팅
│ └── app.html # HTML 템플릿
├── static/ # 정적 파일
├── package.json # 프로젝트 설정 및 의존성
├── svelte.config.js # SvelteKit 설정
├── vite.config.js # Vite 설정
└── .gitignore # Git 제외 파일 목록


---

## ⚙️ 설치 및 실행 방법

### 1) 레포지토리 클론
```bash
git clone https://github.com/2025-UCSI-Internship/yellowsocks-frontend.git
cd yellowsocks-frontend

2) 의존성 설치
npm install

3) 개발 서버 실행
npm run dev


브라우저에서 http://localhost:5173 접속

📜 기여 방법

새로운 브랜치 생성:

git checkout -b feature/기능명


기능 구현 및 커밋:

git commit -m "feat: 기능 설명"


원격 브랜치 푸시 후 PR 생성:

git push origin feature/기능명

👥 팀원

프론트엔드 개발: Junseong Park

백엔드 개발: 팀원 이름

프로젝트 매니저: 팀원 이름
