# CAREER_DOT_ZIP

## 레포지토리 구조

이 레포지토리는 메인 레포로, frontend와 backend를 서브모듈로 관리합니다.

```
CAREER_DOT_ZIP/                         ← 메인 레포 (현재 레포)
  ├── CAREER_DOT_ZIP_BACKEND/           ← 서브모듈 (백엔드)
  ├── CAREER_DOT_ZIP_FRONTEND/          ← 서브모듈 (프론트엔드)
  ├── .gitmodules                        ← 서브모듈 연결 정보
  └── README.md
```

### 기술 스택

| 구분 | 기술 |
|------|------|
| Frontend | React |
| Backend | Python, Django REST Framework |
| 배포 | AWS EC2 |
| CI/CD | GitHub Actions |

---

## 로컬 세팅 방법

### 1. 처음 클론할 때 (권장)

메인 레포 하나만 클론하면 frontend, backend 코드까지 한 번에 받아집니다.

```bash
git clone --recurse-submodules https://github.com/[org]/CAREER_DOT_ZIP.git
```

### 2. 이미 클론한 상태에서 서브모듈 업데이트가 필요할 때

```bash
git submodule update --remote
```

---

## 작업 방법

> 실제 코드 작업은 각 서브모듈 레포에서 진행합니다.

### Frontend 작업

```bash
cd CAREER_DOT_ZIP_FRONTEND
# 작업 후
git add .
git commit -m "커밋 메시지"
git push
```

### Backend 작업

```bash
cd CAREER_DOT_ZIP_BACKEND
# 작업 후
git add .
git commit -m "커밋 메시지"
git push
```

### 서브모듈 변경사항 메인 레포에 반영

서브모듈에서 push 후, 메인 레포도 업데이트해줘야 합니다.

```bash
cd ..  # 메인 레포로 이동
git add CAREER_DOT_ZIP_BACKEND  # 또는 CAREER_DOT_ZIP_FRONTEND
git commit -m "chore: update submodule"
git push
```

---

## 주의사항

- 메인 레포(`CAREER_DOT_ZIP`)에서 직접 소스코드를 수정하지 마세요.
- 팀원이 서브모듈을 업데이트하면 `git submodule update --remote` 로 동기화해주세요.
- 서브모듈 레포(`CAREER_DOT_ZIP_BACKEND`, `CAREER_DOT_ZIP_FRONTEND`)는 각각 독립된 레포입니다.
