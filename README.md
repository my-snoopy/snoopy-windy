# snoopy-windy · GPX 바람 분석

GPX 코스의 **시간대별 바람·고도·속도·날씨·경사**를 분석하고, 라이딩 중에는 **현재 위치 + 풍 흐름 + 진행 방향 회전**을 실시간으로 보여주는 단일 HTML 웹앱.

- 단일 파일 (`wind.html`) — 의존성 없이 동작
- 풍 데이터: Open-Meteo (ECMWF IFS) — 무료·무키
- 지도: Leaflet + OpenStreetMap / CartoDB
- 모바일 PWA 지원 (홈 화면 설치 가능)

---

## 1. GitHub에 올리기 (5분)

### 1-1. 터미널 (macOS / Linux)

`gh` CLI가 설치되어 있을 때 가장 빠릅니다.

```bash
cd /Users/user/Documents/GitHub/wind-route
git init
git add wind.html README.md
git commit -m "snoopy-windy 초기 배포"
gh repo create wind-route --public --source=. --push
gh api -X POST repos/{owner}/wind-route/pages \
  -f source[branch]=main \
  -f source[path]=/
```

`gh` 미설치 시 → https://cli.github.com 에서 설치 (`brew install gh`).

### 1-2. 웹 UI (CLI 없이)

1. https://github.com/new 접속 → 새 repo 이름 `wind-route`, **Public** 선택, "Create repository"
2. 생성된 repo 페이지에서 **"uploading an existing file"** 링크 클릭
3. `wind.html` 과 `README.md` 드래그 → 페이지 하단의 **Commit changes**
4. **Settings → Pages**
   - Source: `Deploy from a branch`
   - Branch: `main` / `/ (root)` 선택 → **Save**
5. 1~2분 후 페이지 상단에 URL 표시:
   ```
   https://<github-id>.github.io/wind-route/wind.html
   ```

### 1-3. URL 형태

| 파일명 | 접속 URL |
|---|---|
| `wind.html` 그대로 | `https://<id>.github.io/wind-route/wind.html` |
| `index.html`로 변경 시 | `https://<id>.github.io/wind-route/` (더 깔끔) |

> 더 짧은 URL을 원하면 `wind.html`을 `index.html`로 이름만 변경 후 다시 push 하세요.

### 1-4. 코드 수정 후 갱신

```bash
git add wind.html
git commit -m "수정 메시지"
git push
```

1~2분 후 GitHub Pages가 자동 반영합니다.

---

## 2. iPhone에 설치하기 (Safari)

> **반드시 Safari로 접속하세요.** Chrome iOS는 PWA 설치를 정식 지원하지 않습니다.

1. **Safari 열기**
2. 주소창에 GitHub Pages URL 입력 (예: `https://<id>.github.io/wind-route/wind.html`)
3. 페이지가 로드되면 하단 가운데 **공유 버튼 ↗** 탭
4. 메뉴에서 **"홈 화면에 추가"** 선택
5. 이름을 `snoopy-windy`로 두고 (또는 원하는 대로) **추가** 탭
6. 홈 화면에 **파란 사각형 + 흰 곡선 3개** 아이콘이 생깁니다
7. 아이콘 탭하면 풀스크린 앱처럼 실행됩니다

### iOS 권한 허용

처음 사용 시 두 가지 권한을 묻습니다:

| 기능 | 권한 | 첫 사용 시점 |
|---|---|---|
| **현재 위치 / 추적** | 위치 (GPS) | `📍 현재위치 기준` 또는 `🛰 추적 시작` 클릭 시 |
| **나침반 회전** | 동작 및 방향 | `🧭 나침반` 클릭 시 |

각 다이얼로그에서 **허용**을 선택하세요. 거부했다면:
- 설정 → Safari → 위치 → 허용 / 거부
- 설정 → Safari → 동작 및 방향 접근 → 허용

> iOS 13 이상 필요. 16+ 권장.

---

## 3. Android(갤럭시 등)에 설치하기 (Chrome)

> Samsung Internet 브라우저도 가능하지만 Chrome이 가장 매끄럽습니다.

1. **Chrome 열기**
2. 주소창에 GitHub Pages URL 입력
3. 잠시 후 화면 하단에 **"앱 설치"** 배너가 자동으로 뜸 → 탭
   - 안 뜨면: 우측 상단 **⋮(메뉴)** → **"앱 설치"** 또는 **"홈 화면에 추가"**
4. 확인 다이얼로그에서 **설치** 탭
5. 홈 화면 + 앱 서랍에 **snoopy-windy** 아이콘 생성
6. 아이콘 탭하면 풀스크린 앱처럼 실행

### Android 권한 허용

- `📍 현재위치` / `🛰 추적` 첫 클릭 시 위치 권한 다이얼로그 → **정확한 위치 허용**
- `🧭 나침반` 첫 클릭 시 동작 센서 권한 → 허용
- 거부했다면: 앱 정보 → 권한 → 위치/센서 다시 허용

---

## 4. 사용법 요약

### 4-1. 데스크탑·모바일 공통

1. **GPX 파일** 선택 (자전거 컴퓨터·Strava·Komoot 등에서 export)
2. **출발 시각** 설정 (오늘~+16일 가능)
3. **방향** 선택: 정방향 / 역방향 / 왕복
4. **입력 모드** 선택: 평속 또는 파워 (체중 포함)
5. **휴식** 입력: 분/회 + 간격 km
6. **분석** 클릭 → 풍 데이터 조회 (10~30초)
7. 색상으로 표시된 경로·통계·게이지·고도+속도 그래프 확인

### 4-2. 라이딩 중 (모바일)

| 버튼 | 동작 |
|---|---|
| 📍 **현재위치 기준** | 현재 GPS 위치를 줌 17로 표시 + 풍 애니메이션. GPX 있으면 가까운 점부터 분석 |
| 🛰 **추적 시작** | GPS 위치 변화에 따라 지도가 자동 따라감, **진행 방향이 위쪽**(heading-up). 다시 누르면 중지 |
| 🧭 **나침반** | 폰 자기 센서로 회전 (정지 시에도 폰 돌리면 따라옴). 다시 누르면 끔 |
| 📊 **그래프 숨김** | 하단 그래프를 숨겨 지도 확장. 다시 누르면 표시 |
| ⏱ **최적 시각** | ±6시간 중 TAILWIND % 가장 큰 시각 자동 적용 |
| 🤖 **AI 프롬프트** | ChatGPT/Claude에 붙여넣을 코스 분석 프롬프트 생성 → 클립보드 복사 |
| 🌬 **snoopy-windy 로고** | 클릭하면 입력 폼 접기/펼치기 토글 (모바일 화면 정리) |

**라이딩 중 자동 화면 정리**
- 모바일에서 분석을 시작하거나 🛰 추적을 켜면 입력 폼이 자동으로 접힘
- 추적/접기 모드에서는 [분석/현재위치/최적시각/AI 프롬프트] 버튼이 자동 숨김 → 라이딩에 필요한 [추적/나침반/그래프 토글]만 남음
- 다시 입력하려면 좌측 상단 **🌬 snoopy-windy** 로고 탭하면 펼쳐짐

### 4-3. 풍 색상 의미 (경로·그래프·현재위치 마커 공통)

| 색상 | 풍 효과 |
|---|---|
| **#1d4ed8** 진한 파랑 | 강한 순풍 (score≥0.5, 풍속≥8km/h) |
| **#3b82f6** 파랑 | 순풍 (score≥0.15) |
| **#64748b** 슬레이트 | 측풍/중립 |
| **#ef4444** 빨강 | 역풍 (score≤-0.15) |
| **#b91c1c** 진한 빨강 | 강한 역풍 (score≤-0.5, 풍속≥8km/h) |

### 4-4. 그래프 인터랙션 (데스크탑·모바일 공통)

- **호버/탭**: 해당 지점의 거리·고도·경사·속도·풍·날씨·기온·도착시각 표시
- **드래그**: 영역 선택 → 지도가 그 구간으로 확대
- **더블클릭/더블탭**: 전체 경로 보기로 줌 리셋
- **휠 (데스크탑)**: 마우스 위치 중심으로 지도 줌

---

## 5. 트러블슈팅

### "현재 위치를 가져올 수 없습니다"
- HTTPS(GitHub Pages 등)로 접속했는지 확인 — `file://`이나 `http://`에서는 GPS 거부됨
- 브라우저 위치 권한이 차단되어 있는지 확인
- iOS는 Safari, Android는 Chrome / Samsung Internet 권장

### "GPS 첫 응답이 너무 늦다"
- 실내거나 GPS 신호 약한 곳 → 야외에서 다시 시도
- 브라우저가 와이파이/셀룰러 위치만 사용 중일 수 있음 — `enableHighAccuracy: true` 옵션 적용되어 있어 GPS 우선이지만 첫 fix는 5~30초 걸릴 수 있음

### "지도 회전이 부자연스럽다"
- 자기 센서 정확도 낮은 환경(차량·실내·자석) → 나침반 끄고 추적만 사용
- 안드로이드: 설정 → 위치 → 정확도 향상(Wi-Fi/Bluetooth 스캔 허용)

### "분석 시 풍 데이터가 일부 비어 있다"
- 출발 시각이 16일 초과 미래거나 2일 초과 과거 → Open-Meteo forecast 범위 밖
- 일시적 API 오류 → 잠시 후 재시도

### "PWA 설치 메뉴가 안 보인다"
- HTTPS 환경인지 확인
- Manifest가 정상 로드되었는지: 데스크탑 Chrome 개발자도구 → Application → Manifest 탭에서 확인

### "라이딩 중 화면이 꺼지면 추적이 멈춘다"
- 모바일 OS가 백그라운드 GPS를 차단함
- 설정 → 디스플레이 → 화면 자동 꺼짐 시간 길게 또는 라이딩 모드(Always-on)
- 화면을 다시 켜면 자동 재개

### "오프라인에서 동작 안 됨"
- Service Worker 미적용이라 인터넷 필수
- 지도 타일·풍 데이터·Leaflet 모두 외부 호출
- 오프라인 지원이 필요하면 `service-worker.js` 추가 필요 (요청 시 작업 가능)

---

## 6. 데이터 출처 / 라이선스

- **풍·기온·강수·고도**: [Open-Meteo](https://open-meteo.com/) (CC BY 4.0)
- **지도 타일**: [OpenStreetMap](https://www.openstreetmap.org/copyright) + [CartoDB Positron](https://carto.com/attribution)
- **지도 라이브러리**: [Leaflet 1.9.4](https://leafletjs.com/) (BSD 2-Clause)
- 사용자 GPX 파일은 **서버에 업로드되지 않으며**, 브라우저 메모리에서만 처리됩니다 (사생활 안전)

---

## 7. 참고 / 한계

- 풍 데이터는 ECMWF 예보 기준이라 실측과 ±1~3 m/s 정도 차이 있을 수 있음
- 경사도는 ±50m 윈도우로 smoothing되어 GPS 노이즈 영향 최소화
- 휴식 시각이 hourly 단위 풍 매핑에 영향을 주어 30분 전후 차이로 다른 hour 데이터로 매핑될 수 있음 (자동 처리됨)
- 파워→속도 모델은 평지 단순 모델 (Crr=0.005, CdA=0.32, ρ=1.225) 기준
