# 가계부 API — 클라우드 컴퓨팅 실습 Week 4

- GitHub: https://github.com/qkryung/ledger-api
- Render: 새 서비스 생성 및 연동 검증 후 주소 기입 예정

## 진행 상태

워크북 4단계까지의 FastAPI·SQLAlchemy 코드가 구현되어 있습니다. 로컬 설정으로 Supabase PostgreSQL에 접속하고 `accounts`, `categories`, `transactions` 테이블을 읽을 수 있음을 확인했습니다.

**이 저장소용 Render 서비스를 새로 만들어야 합니다.** 아래 5단계 배포와 Supabase 저장·조회 확인을 마친 뒤 새 서비스 주소를 README에 기입하고 제출합니다.

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

## 남은 필수 작업: 5단계 Render + Supabase

1. Render에서 **New → Web Service**로 새 서비스를 만들고 이 저장소의 `main` 브랜치를 연결합니다. 저장소 루트에 코드가 있으므로 Root Directory는 비워 둡니다.
2. Build Command를 `pip install -r requirements.txt`로 설정합니다.
3. Start Command를 `uvicorn main:app --host 0.0.0.0 --port $PORT`로 설정합니다.
4. Render의 Environment에 `DATABASE_URL`을 추가하고 로컬 `.env`의 Supabase 연결 문자열 **값**을 입력합니다. 환경변수가 없으면 이 코드는 SQLite로 전환되므로 반드시 설정합니다.
5. 새 서비스의 배포가 완료되면 발급된 Render 주소의 `/docs`를 엽니다. Health Check Path를 설정하는 경우 이 코드에 존재하는 `/accounts`를 사용합니다.
6. Render의 `POST /accounts`에서 테스트 계좌를 생성하고, `GET /accounts`로 조회합니다. Supabase Table Editor의 `accounts`에도 같은 ID·이름·잔액이 나타나는지 확인합니다.
7. 이 README의 Render 주소를 검증 완료한 주소로 확정하고, GitHub 주소와 Render 주소를 제출합니다.

워크북 6·7단계는 평가 제외 확장 실습입니다.

## 확인한 범위

- 임시 SQLite DB에서 계좌 생성·조회, 거래 생성, 중첩 응답, 지출 집계 및 없는 계좌의 404 응답을 검증했습니다.
- 로컬 Supabase 연결은 읽기 전용으로 확인했습니다. 원격 DB에 테스트 데이터를 추가하지 않았습니다.
- Render에서 Supabase로 데이터를 저장하는 검증은 남아 있습니다.
