# Workflow assist

논문·보고서용 워크플로우 / 플로우차트를 **규격(그리드·간격·대칭 연결선)은 자동으로 지키되, 모든 요소는 자유롭게 조정**할 수 있게 그리는 웹 에디터입니다. 서버 없이 `index.html` 한 파일로 동작합니다.

- 앱: `index.html`
- 사용 설명서: `manual.html` (앱의 "단축키" 창에서도 열립니다)
- 데이터는 브라우저(localStorage)에 자동 저장되고, SVG / PNG / JSON으로 내보낼 수 있습니다.

## 주요 기능

- **규격 자동 유지** — 그리드 스냅, 표준 간격, 대칭 직각 연결선, 이동 시 중심선 자석 가이드
- **자유로운 조정** — 위치·크기·글꼴·색을 드래그 또는 수치 입력으로 override
- **연결선** — 포트 드래그로 생성(빈 곳에 놓으면 정렬된 새 노드 자동 생성), 끝점 재연결, 꺾임점 이동, 길이 통일
- **정렬 도구** — 정렬 6종, 균등 분배, 간격 고정, 너비·높이 길이 맞춤, 그룹(Ctrl+G)
- **도형 13종** — 프로세스·판단·데이터·육각형·화살 블록·별·문서·클라우드 등
- **손그림 인식** — 대충 그린 도형·선을 규격 도형으로 변환하고, 틀리면 즉시 다른 도형으로 전환
- **이미지 라이브러리** — PNG/JPG/SVG 업로드 후 도형처럼 재사용, 배경 제거(누끼), 회색조 변환
- **학술지 스타일 프리셋** — Nature / Cell / Mono, Okabe–Ito 계열 팔레트 + 사용자 색 추가
- **예제 8종** — 논문용(실험 워크플로우, CONSORT, PRISMA 2020, 연구 설계, ML 파이프라인), 보고서용(승인 프로세스, Phase-gate, 데이터 흐름도)

## 로컬에서 실행

파일을 더블클릭해 열어도 되지만, 한글 폰트와 이미지 업로드를 안정적으로 쓰려면 간단한 정적 서버로 여는 것을 권장합니다.

```bash
python -m http.server 5173
```

브라우저에서 `http://localhost:5173` 을 엽니다.

## 웹에 올리기 (배포)

정적 파일(`index.html`, `manual.html`)만 있으면 되므로 아래 어느 방법이든 무료로 가능합니다.

### 1) GitHub Pages — 무료, 주소가 안정적 (추천)

git을 설치하지 않아도 웹 브라우저만으로 됩니다.

1. https://github.com/new 에서 저장소를 만듭니다 (예: `workflow-assist`, **Public**).
2. 저장소 화면의 **Add file → Upload files**에 `index.html`, `manual.html`, `README.md`를 끌어다 놓고 **Commit changes**.
3. **Settings → Pages → Build and deployment**: Source = *Deploy from a branch*, Branch = *main* / *(root)* → Save.
4. 1~2분 뒤 `https://<계정>.github.io/workflow-assist/` 로 접속됩니다. 파일을 수정하면 같은 방법으로 다시 업로드(덮어쓰기)하면 됩니다.

git 또는 [GitHub Desktop](https://desktop.github.com/)을 쓴다면 이 폴더를 저장소로 열어 커밋 → Publish 하면 같은 결과입니다.

### 2) Netlify Drop — 드래그 앤 드롭

1. https://app.netlify.com/drop 에 접속합니다.
2. 이 폴더(또는 `index.html`, `manual.html`)를 브라우저 창에 끌어다 놓습니다.
3. 바로 `https://<임의이름>.netlify.app` 주소가 생깁니다. Site settings에서 이름을 바꿀 수 있습니다.

### 3) Cloudflare Pages / Vercel

- Cloudflare Pages: Workers & Pages → Create → **Upload assets**로 폴더 업로드, 또는 GitHub 저장소 연결(빌드 명령 없음, 출력 디렉터리 `/`).
- Vercel: `npx vercel` 을 이 폴더에서 실행하거나 GitHub 저장소를 가져오기(Framework: Other).

### 사용자 지정 도메인

세 서비스 모두 설정 화면에서 도메인을 연결할 수 있습니다(DNS에 CNAME 추가). HTTPS는 자동으로 적용됩니다.

## 파일 구조

```
index.html           앱 전체 (HTML + CSS + JS, 외부 의존성 없음) — 이 파일이 곧 소스코드
manual.html          사용 설명서
embed-example.html   다른 웹사이트에 iframe으로 넣고 API로 통신하는 예제
DEVELOPER.md         개발자 인수인계 문서 (통합 방법 · 코드 구조 · JSON 형식 · API)
README.md            이 문서
LICENSE              MIT
```

빌드 과정이 없습니다. `index.html`을 열면 그대로 실행되고, 정적 호스팅에 올리면 그대로 서비스됩니다. React/Vite 같은 도구는 필요 없으며, 기존 사이트에 넣는 방법은 `DEVELOPER.md`를 보세요.

## 브라우저 지원

Chrome / Edge / Safari / Firefox 최신 버전. 이미지 업로드, 자동 저장, 사용자 색·이미지 라이브러리는 브라우저별로 따로 저장됩니다(다른 PC로 옮기려면 JSON 내보내기를 사용하세요).

## 라이선스

MIT — `LICENSE` 참고.
