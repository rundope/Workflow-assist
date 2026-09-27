# Workflow assist — 개발자 인수인계 문서

다른 개발자가 이 도구를 자기 웹사이트에 붙이거나 수정할 때 필요한 모든 정보입니다.

## 1. 한눈에

| 항목 | 내용 |
|---|---|
| 기술 스택 | 순수 HTML + CSS + JavaScript (ES2020). **프레임워크·빌드 도구·npm 없음** |
| 소스 코드 | `index.html` 한 파일이 소스이자 완성품입니다 (별도 소스 없음) |
| 외부 의존성 | Google Fonts 링크 1개 (없어도 동작 — 시스템 Arial/Helvetica로 대체) |
| 서버 | 필요 없음. 정적 파일 호스팅이면 어디든 (GitHub Pages, S3, Nginx, 사내 웹서버) |
| 데이터 | 브라우저 localStorage에 자동 저장. 서버 저장이 필요하면 §4의 API로 JSON을 꺼내 DB에 넣으면 됩니다 |
| 브라우저 | Chrome / Edge / Safari / Firefox 최신 버전 |
| 라이선스 | MIT |

**전달할 파일**: `index.html`, `manual.html`, `embed-example.html`, `README.md`, `DEVELOPER.md`, `LICENSE`.
`.claude/` 폴더는 개발 중 미리보기 서버 설정일 뿐이므로 전달·배포 대상이 아닙니다.

## 2. "React + TypeScript + Vite가 필요한가?" — 아니오

이 앱은 프레임워크 없이 완결되어 있습니다. 동업자의 사이트가 React/Vue/Next 등 무엇으로 만들어졌든 아래 세 방법 중 하나로 붙일 수 있고, **A가 거의 항상 정답**입니다.

| 방법 | 작업량 | 언제 |
|---|---|---|
| **A. iframe으로 넣고 API로 통신** | 반나절 | 대부분의 경우. 사이트 기술 스택과 무관하고, 에디터 업데이트가 사이트에 영향을 주지 않음 |
| B. 같은 도메인의 정적 경로로 서비스 | 1시간 | 그냥 `/tools/workflow/` 같은 주소로 열어주기만 하면 될 때 |
| C. React 컴포넌트로 포팅 | 수 주 | 사이트의 상태 관리·디자인 시스템과 완전히 한 몸이 되어야 할 때. 사용자 입장에선 이득이 없으므로 **권장하지 않음** |

### A. iframe + API (권장)

```html
<iframe id="editor" src="/tools/workflow-assist/index.html" style="width:100%;height:700px;border:0"></iframe>
<script>
  const editor = document.getElementById('editor').contentWindow;
  // 문서 넣기
  editor.postMessage({ target: 'workflow-assist', type: 'load', doc: savedJson, requestId: 'a1' }, '*');
  // 문서 꺼내기
  editor.postMessage({ target: 'workflow-assist', type: 'getDoc', requestId: 'a2' }, '*');
  window.addEventListener('message', (ev) => {
    if (ev.data?.source !== 'workflow-assist') return;
    if (ev.data.type === 'doc')    save(ev.data.doc);          // JSON → 서버
    if (ev.data.type === 'change') markDirty();                // 자동 저장 시점마다 발생
  });
</script>
```

완전한 동작 예제는 `embed-example.html`입니다 (같은 폴더에서 열면 바로 동작).

같은 도메인(same-origin)이면 postMessage 없이 직접 호출도 됩니다:
```js
const api = document.getElementById('editor').contentWindow.WorkflowAssist;
api.load(doc); const doc = api.getDoc(); const svg = api.exportSVG();
```

### B. 정적 경로

`index.html`, `manual.html`을 사이트의 정적 폴더(`public/`, `static/`, `wwwroot/` 등)에 복사하고 링크를 겁니다. 끝.

### C. React 포팅 시 참고

포팅한다면 상태(`state`)는 그대로 쓰고, `renderFigure()`의 SVG 생성 로직을 JSX로, `inspector`의 innerHTML 템플릿을 컴포넌트로 옮기는 순서가 자연스럽습니다. 라우팅(`routeEdge`)·인식(`recognize`)·정렬(`alignSel`, `distribute`)은 순수 함수라 그대로 재사용됩니다.

## 3. index.html 코드 구조

파일 안에서 `/* ---------- 이름 ---------- */` 주석으로 구역이 나뉩니다. (줄 번호는 2026-09-17 기준)

| 줄 | 구역 | 내용 |
|---|---|---|
| 1–260 | `<style>` | UI 크롬 스타일. CSS 변수로 라이트/다크 테마 |
| 262–330 | HTML 마크업 | 상단 바, 왼쪽 툴바, 캔버스 `<svg>`, 오른쪽 인스펙터, 상태줄 |
| 277 | constants | `FONTS`, `HUES`/`FILLS`/`STROKES`(팔레트), `SHAPES`, `PRESETS`, `defaultStyle()`, 사용자 색·이미지 라이브러리(localStorage) |
| 330 | state | 문서 상태 `state`, 선택 `sel`, 뷰 `view`, 현재 도구 `tool` |
| 348 | templates | `TEMPLATES.{rnaseq,consort,prisma,studydesign,ml,approval,phases,dataflow}` — 예제 문서 생성 함수 |
| 492 | history / persistence | undo/redo 스택(`snapshot`/`undo`/`redo`), `mutate(fn)`, `persist()` |
| 502 | geometry | 포트 좌표, 자동 포트 선택(`resolveEnds`), **직각 연결선 라우터 `routeEdge`**, 둥근 모서리 경로 |
| 576 | rendering | 도형 SVG 생성 `nodeShapeEl`, 노드/밴드/텍스트/연결선 렌더, 오버레이(핸들·포트·가이드) |
| 722 | view | viewBox 기반 확대/축소/이동, `fitPage` |
| 740 | tools | 도구 전환 `setTool`, 도형 추가 `addNodeAt`, 연결선 생성 |
| 753 | smart guides | 드래그 중 중심선·모서리 스냅 `smartSnap` |
| 760 | pointer interaction | 단일 `pointerdown/move/up` 상태 머신 (`drag.mode`: move / resize / connect / reattach / bend / marquee / ink / pan) |
| 833 | inline editing | 더블클릭 텍스트 편집 (textarea 오버레이) |
| 850 | sketch recognition | 손그림 → 도형 인식 (`rdp` 단순화 + 모서리 수 + 축/대각선 비율) |
| 895 | actions | 삭제·복제·정렬·분배·크기 맞춤·길이 통일·그룹·프리셋 |
| 911 | inspector | 오른쪽 패널 HTML 생성 `renderInspector` + 위임 이벤트 핸들러 (`data-action`, `data-prop`) |
| 1047 | menus / export | 파일·내보내기 메뉴, `buildExportSVG`, PNG 변환, JSON 입출력, 배경 제거 |
| 1076 | keyboard | 단축키 |
| ~1096 | embed API | `window.WorkflowAssist` + postMessage 브리지 (§4) |
| 마지막 | boot | localStorage 복원 → 없으면 예제 로드 → 렌더 |

**코드 스타일 메모**: 한 줄에 여러 문장을 쓴 압축된 스타일입니다. 읽기 편하게 만들려면 Prettier를 한 번 돌리면 됩니다: `npx prettier --write index.html` (동작은 바뀌지 않습니다).

**렌더링 원칙**: 상태를 바꾼 뒤 `renderCanvas()`(그림만) 또는 `renderAll()`(그림 + 인스펙터)을 호출하는 단순한 "전체 다시 그리기" 모델입니다. 가상 DOM은 없지만 수백 개 요소까지는 충분히 빠릅니다.

## 4. API (임베드용)

에디터 창에는 `window.WorkflowAssist`가 있고, 다른 origin에서는 postMessage로 같은 기능을 씁니다.

### JS API (same-origin)

| 메서드 | 설명 |
|---|---|
| `load(doc)` | JSON 문서를 불러옴 (§5 형식). 잘못된 형식이면 throw |
| `getDoc()` | 현재 문서의 깊은 복사본을 반환 |
| `exportSVG()` | 내보내기용 SVG 문자열 (그리드·선택 표시 없음, 폰트 지정 포함) |
| `loadTemplate(id)` / `templates()` | 예제 불러오기 / 예제 id 목록 |
| `newDocument()` | 빈 문서 |
| `on('change', fn)` | 자동 저장 시점마다 호출. 해제 함수를 반환 |

### postMessage 프로토콜

보내기: `{ target: 'workflow-assist', type, requestId?, ...payload }`
받기: `{ source: 'workflow-assist', type, requestId, ...payload }`

| 보내는 type | payload | 응답 type |
|---|---|---|
| `load` | `doc` | `loaded` |
| `getDoc` | — | `doc` (`{doc}`) |
| `exportSVG` | — | `svg` (`{svg}`) |
| `loadTemplate` | `id` | `loaded` |
| `newDocument` | — | `loaded` |
| `templates` | — | `templates` (`{templates:[...]}`) |
| (오류 시) | | `error` (`{message}`) |

에디터가 스스로 보내는 이벤트: `{ source:'workflow-assist', type:'change' }` — 부모 창으로 자동 저장 시점마다 전송.

PNG가 필요하면 `exportSVG()` 결과를 서버에서 변환하거나(예: `sharp`, `resvg`, Inkscape CLI), 브라우저에서 `<canvas>`에 그려 `toBlob`하면 됩니다 (`exportPNG()` 참고).

## 5. 문서 JSON 형식

```jsonc
{
  "page":  { "w": 1200, "h": 680 },                 // 캔버스(=내보내기) 크기, px
  "style": {                                        // 그림 전체 규격
    "font": "helvetica",                            // FONTS id: helvetica | notosans | plex | times
    "fontSize": 12, "textColor": "#1A1A1A",
    "stroke": "#1A1A1A", "strokeWidth": 1.2,        // 기본 선 색·굵기
    "radius": 4, "arrowSize": 8, "gap": 48, "grid": 10,
    "bandFill": "#F4F5F7", "preset": "nature"       // nature | cell | mono
  },
  "elements": [ /* 아래 4종 */ ],
  "edges":    [ /* 연결선 */ ],
  "groups":   [ { "id": "g1", "members": ["n1","n2","e1"] } ]
}
```

요소 4종 (`elements[]`):

```jsonc
// 노드 — 필수: id,type,shape,x,y,w,h,label
{ "id":"n1", "type":"node", "shape":"rounded", "x":100, "y":100, "w":140, "h":48,
  "label":"두 줄은\n\\n으로", "fill":"#DCE8F6",
  "stroke":"#2F6DB5",        // 생략 시 style.stroke, "none" 이면 테두리 없음
  "strokeWidth":1.2, "fontSize":12, "bold":false, "align":"left", "textColor":"#1A1A1A",
  "src":"data:image/png;base64,...", "mono":true, "frame":false, "srcOriginal":"..."  // shape:"image" 전용
}
// 단계 밴드 (항상 노드 뒤에 그려짐)
{ "id":"b1", "type":"band", "x":40, "y":40, "w":1120, "h":170,
  "label":"a  Sample collection",   // 첫 글자 + 공백 2칸 → 패널 문자로 굵게
  "fill":null, "radius":6, "border":"none" }         // border: none | solid | dashed
// 텍스트
{ "id":"t1", "type":"text", "x":60, "y":60, "label":"Figure 1", "fontSize":14, "bold":true, "color":"#1A1A1A" }
// 손그림 원본 (인식하지 않고 남긴 스케치)
{ "id":"k1", "type":"ink", "pts":[{"x":1,"y":2}, ...], "stroke":"#1A1A1A" }
```

`shape` 값: `rect rounded pill diamond ellipse parallelogram cylinder hexagon chevron triangle star document cloud image`

연결선 (`edges[]`):

```jsonc
{ "id":"e1",
  "from": { "node":"n1", "port":"auto" },   // port: auto | t | r | b | l   /  노드 없이 { "x":.., "y":.. } 자유 끝점도 가능
  "to":   { "node":"n2", "port":"l" },
  "route":"ortho",                          // ortho | straight | curve
  "startHead":"none", "endHead":"arrow",    // none | arrow | open | dot
  "dash":false, "label":"Yes",
  "bend":58,                                // 직각 경로 꺾임 위치 오프셋(px). 생략 시 자동 대칭
  "stroke":"#1A1A1A", "strokeWidth":1.2, "fontSize":11 }
```

id는 문서 안에서만 유일하면 되는 문자열입니다. 좌표 단위는 캔버스 px (SVG user unit).

## 6. 브라우저 저장소 키

| 키 | 내용 |
|---|---|
| `figureassist.doc.v3` | 마지막 작업 문서 (JSON) |
| `figureassist.colors.v2` | 사용자 추가 색 `{fill:[],stroke:[],text:[]}` |
| `figureassist.images` | 이미지 라이브러리 `[{id,name,src(dataURL),w,h}]` |

호스트 사이트가 문서를 서버에 저장한다면 `load()`/`getDoc()`만 쓰고 localStorage는 무시해도 됩니다.

## 7. 확장하기

- **도형 추가**: `SHAPES`에 항목(id·라벨·아이콘) 추가 → `nodeShapeEl()`의 `switch`에 SVG 생성 case 추가 → 필요하면 `defaultSize()`·`renderNode()`의 텍스트 위치 보정.
- **예제 추가**: `TEMPLATES`에 함수 추가(`N()`, `E()`, `band()`, `T()` 헬퍼 사용) → 상단 "파일" 메뉴에 `<button data-act="tpl:아이디">` 추가.
- **프리셋 추가**: `PRESETS`에 항목 추가 (글꼴·선 굵기·모서리·화살표·밴드색).
- **글꼴 추가**: `FONTS`에 항목 추가 + `<link>`의 Google Fonts 목록에 패밀리 추가. 한글이 있으면 KR 계열 폰트를 쓰세요.
- **팔레트 변경**: `HUES`(채움 계열), `STROKES`(선·글자), `EXTRA_COLORS`(추가 팝업의 후보색).
- **UI 문구/언어**: 마크업과 `renderInspector()`의 템플릿 문자열에 직접 있습니다. i18n 층은 없습니다.

## 8. 알려진 제약과 개선 후보

- 손그림 인식은 규칙 기반(단순화 → 모서리 수 → 축/대각선 비율)입니다. 정확도를 높이려면 `recognize()`를 $P/$1 인식기나 작은 모델로 교체하면 됩니다.
- 이미지 "배경 제거"는 가장자리에서 이어진 단색 배경만 처리합니다(플러드 필). 사진에는 서버 측 세그멘테이션이 필요합니다.
- PNG 내보내기는 브라우저 `<canvas>`로 그리므로 시스템 글꼴이 쓰입니다. 정확한 글꼴이 필요하면 SVG를 서버에서 변환하세요.
- 다중 사용자 동시 편집, 서버 저장, 로그인은 범위 밖입니다 — 호스트 사이트가 §4 API로 감싸는 구조를 전제로 합니다.
- 접근성: 키보드 단축키는 있으나 스크린리더용 ARIA는 최소한입니다.

## 9. 로컬 실행·테스트

```bash
python -m http.server 5173     # 또는 npx serve, 아무 정적 서버
```
`http://localhost:5173/` (에디터), `http://localhost:5173/embed-example.html` (통합 예제), `http://localhost:5173/manual.html` (설명서).
자동화 테스트는 없습니다. 수동 확인 목록: 노드 드래그 스냅 → 포트 드래그로 새 노드 → 더블클릭 편집 → 다중 선택 정렬 → 손그림 → SVG 내보내기 → 새로고침 후 복원.
