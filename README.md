# ops-infra

담소 서비스 인프라 오케스트레이션

## 구조

```
ops-infra/
├── docker-compose.yml    # 전체 서비스 오케스트레이션
├── Caddyfile             # 리버스 프록시 (HTTPS)
├── livekit.yaml.example  # LiveKit 설정 템플릿
├── api.env.example       # API 환경변수 템플릿
├── web.env.example       # Web 환경변수 템플릿
├── db.env.example        # DB 환경변수 템플릿
└── secrets/              # APNs 인증서 등 (gitignore)
```

## 사전 요구사항

- Docker & Docker Compose
- ops-api, ops-web 레포가 같은 디렉토리에 clone 되어 있어야 함

```
/home/ubuntu/
├── ops-api/
├── ops-web/
└── ops-infra/    # 여기서 docker compose up
```

## 설정

```bash
# 환경 파일 복사
cp api.env.example api.env
cp web.env.example web.env
cp web.env.example .env
cp db.env.example db.env
cp livekit.yaml.example livekit.yaml

# 각 파일 수정하여 실제 값 입력
```

## 실행

```bash
# 전체 서비스 시작
docker compose up -d

# 로그 확인
docker compose logs -f api
docker compose logs -f web

# 서비스 중지
docker compose down
```

## 서비스 포트

| 서비스     | 내부 포트 | 외부 접근              |
| ---------- | --------- | ---------------------- |
| API        | 8080      | Caddy → /v1/\*         |
| Web        | 3000      | Caddy → /              |
| LiveKit    | 7880      | Caddy → /rtc*, /twirp* |
| PostgreSQL | 5432      | 내부만                 |
| Redis      | 6379      | 내부만                 |
| Caddy      | 80, 443   | 외부 HTTPS             |
