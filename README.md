# 가계부 API — 클라우드 컴퓨팅 실습 Week 4

- GitHub: https://github.com/qkryung/ledger-api
- Render: https://dfmba-ledger-api.onrender.com
- Swagger API 문서: https://dfmba-ledger-api.onrender.com/docs

## 진행 상태

워크북 1~4단계 구현과 **5단계 Render 배포·Supabase PostgreSQL 연동 검증을 완료했습니다.**

Render의 FastAPI에서 계좌와 거래를 생성하고, 같은 데이터가 Supabase의 `accounts`, `transactions` 테이블에 저장되는 것을 직접 확인했습니다. 기본 주소(`/`)를 열면 `/docs`로 이동합니다.

## 구현된 기능

| 메서드 | 경로 | 기능 |
| --- | --- | --- |
| POST | `/accounts` | 계좌 생성 |
| GET | `/accounts` | 계좌 목록 |
| GET | `/accounts/{account_id}` | 계좌 단건 조회 |
| POST | `/transactions` | 거래 생성 |
| GET | `/accounts/{account_id}/detail` | 계좌와 거래 목록 중첩 조회 |
| GET | `/stats/by-category` | 카테고리별 지출 합계·건수 |

## 로컬 실행

```powershell
pip install -r requirements.txt
uvicorn main:app --reload
```

프로젝트 폴더의 `.env`에 `DATABASE_URL`을 설정하고 http://127.0.0.1:8000/docs 에서 실행합니다. 현재 드라이버에 맞춰 연결 문자열은 `postgresql+psycopg://`로 시작해야 합니다. `.env`와 DB 비밀번호는 Git에 올리지 않습니다.

## Render 배포 설정

| 항목 | 설정 |
| --- | --- |
| 서비스 이름 | `dfmba-ledger-api` |
| 요금제 / 리전 | Free / Singapore |
| 저장소 / 브랜치 | `qkryung/ledger-api` / `main` |
| Root Directory | 비워 둠 |
| Build Command | `pip install -r requirements.txt` |
| Start Command | `uvicorn main:app --host 0.0.0.0 --port $PORT` |
| 환경변수 | `DATABASE_URL` — Supabase Session Pooler 연결, `sslmode=require` |

실제 연결 문자열은 Render 환경변수에만 저장했습니다. `DATABASE_URL`이 없으면 이 코드는 SQLite로 전환되므로 설정을 유지해야 합니다. `main` 브랜치에 push하면 자동 배포됩니다.

`render.yaml`은 같은 구성을 재현할 때 사용할 수 있는 Blueprint 템플릿입니다. 현재 서비스는 Blueprint로 관리하지 않으므로 이 파일 수정만으로 서비스 설정이 바뀌지는 않습니다. 템플릿은 HTTP `/accounts` 상태 확인을 지정하며, 현재 서비스는 기본 TCP 상태 확인을 사용합니다.

워크북 6·7단계는 평가 제외 확장 실습입니다.

## 연동 검증 결과

2026-10-06에 배포된 Render 주소를 대상으로 확인했습니다.

- `/docs`, `/openapi.json`, `GET /accounts`: 정상 응답.
- `POST /accounts`, `POST /transactions`: 각각 HTTP 201로 생성 성공.
- 계좌 목록, 거래 중첩 상세, 카테고리별 지출 집계: 정상 응답.
- Supabase에 별도로 읽기 전용 접속하여 Render에서 생성한 계좌·거래의 ID와 값이 일치함을 확인.
- 확인용 계좌 ID `1`과 거래 ID `1`을 남겨 두었습니다. 거래 금액은 `-1000`, 메모는 `Deployment integration verification`입니다.

로컬 임시 SQLite DB에서도 주요 기능과 없는 계좌의 404 응답을 검증했습니다.

제출 항목은 맨 위의 GitHub 주소와 Render 주소입니다.
