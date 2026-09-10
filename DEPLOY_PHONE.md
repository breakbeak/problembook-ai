# 📱 휴대폰으로 배포하기

이 ZIP은 **파일을 휴대폰에서 직접 열어 쓰는 앱이 아닙니다.**
서버에 배포한 뒤 생기는 `https://...onrender.com` 주소로 접속해야 버튼과 API 저장이 작동합니다.

## 가장 쉬운 방법

### 1. GitHub에 프로젝트 올리기
휴대폰에서 GitHub에 새 저장소(repository)를 만들고,
이 ZIP을 풀어서 **안의 파일/폴더를 그대로** 저장소 루트에 올립니다.

최종적으로 저장소 첫 화면에 아래가 보여야 합니다.

- `server.py`
- `requirements.txt`
- `render.yaml`
- `templates/`
- `static/`
- `.python-version`

### 2. Render에서 배포
Render Dashboard에서 **New → Web Service**를 선택하고 GitHub 저장소를 연결합니다.
Render의 Flask 배포 방식은 Python 앱에 `pip install -r requirements.txt`를 빌드 명령으로,
Gunicorn을 시작 명령으로 사용할 수 있습니다.

권장 값:
- Build Command: `pip install -r requirements.txt`
- Start Command: `gunicorn server:app`
- Health Check Path: `/health`
- Plan: Free (테스트용)

### 3. 환경변수
Render의 Environment에서 다음을 설정합니다.

- `ADMIN_PASSWORD` = 원하는 관리자 비밀번호
- `ADMIN_SESSION_SECRET` = 긴 랜덤 문자열
- `OPENAI_MODEL` = `gpt-5.6-luna`

**OPENAI_API_KEY는 관리자 페이지에서 저장할 수 있습니다.**
다만 Free 서비스는 로컬 파일/SQLite가 영구 보존되지 않으므로 서버가 재시작되거나 재배포되면
관리자 계정/API 키가 사라질 수 있습니다. 장기 운영에는 영구 데이터베이스가 필요합니다.

### 4. 배포가 끝나면
Render가 만들어 준 주소를 엽니다.

- 사용자: `https://사이트주소.onrender.com/`
- 관리자: `https://사이트주소.onrender.com/admin`

관리자 최초 계정:
- 아이디: `admin`
- 비밀번호: `ADMIN_PASSWORD`에 설정한 값

관리자 페이지에서 API 키를 저장하고 연결 테스트를 합니다.
친구에게는 **관리자 주소가 아니라 `/` 사용자 주소**를 알려주세요.

## ⚠️ 중요
Free Render 웹 서비스는 15분 동안 요청이 없으면 잠들었다가 다음 요청 때 다시 시작할 수 있고,
로컬 파일 변경사항도 재시작/재배포 시 사라집니다. 따라서 이 버전은 **테스트/프로토타입용**입니다.

API 키는 절대로 채팅, GitHub 코드, 친구에게 공개하지 마세요.
