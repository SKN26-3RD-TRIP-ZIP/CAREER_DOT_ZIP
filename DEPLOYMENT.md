# GitHub Actions CI/CD

루트 저장소의 `Build and deploy` 워크플로가 백엔드와 프론트 운영 이미지를
GitHub Container Registry(GHCR)에 빌드·푸시하고, EC2에서는 이미지를 빌드하지
않고 `docker compose pull`로 내려받아 재기동합니다.

## GitHub 설정

`production` Environment에 다음 Secrets를 등록합니다.

- `EC2_HOST`: EC2 호스트 또는 IP
- `EC2_USER`: SSH 사용자
- `EC2_SSH_KEY`: SSH 개인 키

필요하면 다음 Variables도 등록합니다.

- `VITE_API_BASE_URL`: 프론트 빌드 시 API 기준 경로. 미설정 시 `/api/v1`
- `PUBLIC_BASE_URL`: 배포 후 확인할 HTTPS 주소. 예: `https://example.com`

워크플로의 `GITHUB_TOKEN`이 GHCR 푸시와 배포 시점의 EC2 pull 인증에 사용되므로
별도의 장기 GHCR 토큰은 저장하지 않습니다. 저장소의 Actions 설정에서
Workflow permissions가 패키지 쓰기를 허용해야 합니다.

## EC2 전제 조건

- Docker와 Docker Compose plugin이 설치되어 있어야 합니다.
- 배포 디렉터리는 `~/CAREER_DOT_ZIP`입니다.
- 기존 운영 환경 파일은 `~/CAREER_DOT_ZIP/CAREER_DOT_ZIP_BACKEND/.env`에 둡니다.
- 인증서와 호스트 nginx 설정은 기존 위치를 그대로 사용합니다.

배포 시 Actions는 루트의 `docker-compose.prod.yml`만 업로드합니다. `.env`, 인증서,
호스트 nginx 설정, Docker volume은 업로드하거나 삭제하지 않습니다. 프론트 이미지
내부 nginx 설정도 기존 `CAREER_DOT_ZIP_FRONTEND/nginx.conf`를 그대로 빌드합니다.

`main` Pull Request에서는 두 이미지를 빌드해 Dockerfile을 검증하지만 푸시나 EC2
배포는 하지 않습니다. `main` push 또는 수동 실행에서는 커밋 SHA 태그와 `latest`
태그를 푸시한 뒤, EC2에서 SHA 태그를 pull해 배포합니다.

롤백할 때는 정상 동작했던 SHA 12자리로 태그를 지정해 실행합니다.

```sh
BACKEND_TAG=<sha> FRONTEND_TAG=<sha> docker compose -f docker-compose.prod.yml pull backend frontend
BACKEND_TAG=<sha> FRONTEND_TAG=<sha> docker compose -f docker-compose.prod.yml up -d --no-build
```
