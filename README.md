# 뉴욕 겨울 동선 지도

2026.12.27 ~ 2027.01.02 뉴욕 여행 동선을 날짜별로 보여주는 단일 페이지(`index.html`)입니다.
Google Maps JavaScript API를 쓰고, 빌드 도구 없이 브라우저에서 바로 열립니다.

## 구조

```
index.html                     페이지 전체 (데이터·스타일·스크립트)
.github/workflows/deploy.yml   GitHub Pages 배포 (API 키 주입)
```

동선 데이터는 `index.html` 안의 `DAYS`, `LODGES` 배열에 있습니다.

## API 키

저장소의 `index.html`에는 키를 넣지 않습니다(`'YOUR_API_KEY'` 그대로 둠).
배포할 때 GitHub Actions가 저장소 시크릿의 값을 넣어 줍니다.

```bash
gh secret set GOOGLE_MAPS_API_KEY   # 프롬프트에 키 붙여넣기
gh secret set GOOGLE_MAP_ID         # (선택) 직접 만든 Map ID. 없으면 DEMO_MAP_ID 사용
```

Google Cloud 콘솔에서 키 제한을 꼭 걸어 주세요.

- API 제한: **Maps JavaScript API**, **Routes API** (실제 도보·지하철 경로에 필요)
- 웹사이트 제한: `https://<GitHub 사용자명>.github.io/*`

## 실제 경로 (Routes API)

장소 사이 이동은 Routes API로 계산합니다. 1.25km 이하는 도보, 그 외는 지하철(없으면 다른 대중교통)이고,
장소 데이터에 `via`(이동 수단), `through`(경유 좌표), `alt`(대안 수단)를 넣어 바꿀 수 있습니다.
숙소 복귀 시각은 각 날짜의 `back`, 공휴일 시간표는 `svcDate`로 정합니다.
계산 결과는 브라우저에 7일간 저장돼 같은 날짜를 다시 열어도 API를 다시 부르지 않습니다.

노선별 24시간 운행 여부(첫차·막차)는 `index.html`의 `SUBWAY_24H`, `SUBWAY_PART` 표를 따릅니다.

## 배포

`main` 브랜치에 푸시하면 GitHub Pages로 자동 배포됩니다.
수동 실행은 Actions 탭 → **Deploy to GitHub Pages** → *Run workflow*.

## 로컬 미리보기

`index.html`을 브라우저로 열면 됩니다. 키가 없으면 간이 지도로 표시됩니다.
