# Workflow-assist

**[English](#english) · [한국어](#한국어) · [日本語](#日本語) · [中文](#中文)**

---

## English

A web editor for drawing journal-grade workflows and flowcharts for papers and reports. **The rules — grid, spacing, symmetric connectors — are kept automatically, while every element stays freely adjustable.** It runs from a single `index.html` file with no server or build step.

- **Live demo: [https://rundope.github.io/Workflow-assist/](https://rundope.github.io/Workflow-assist/)** (`index.html`)
- User guide: [`manual.html`](https://rundope.github.io/Workflow-assist/manual.html) (opens the guide in your browser's language — [English](https://rundope.github.io/Workflow-assist/manual_en.html) · [한국어](https://rundope.github.io/Workflow-assist/manual_kr.html) · [日本語](https://rundope.github.io/Workflow-assist/manual_jp.html) · [中文](https://rundope.github.io/Workflow-assist/manual_cn.html))
- Work is auto-saved in the browser (localStorage) and can be exported as SVG / PNG / JSON.

> **Interface language:** English and Korean — switch with the globe button in the top bar. The editor starts in English unless your browser is set to Korean.

### Features

- **Rules kept automatically** — grid snapping, standard spacing, symmetric orthogonal connectors, magnetic center-line guides while dragging
- **Free adjustment** — override position, size, font and color by dragging or typing values
- **Connectors** — drag from a port to create one (drop on empty space to create a new aligned node), reconnect endpoints, move bends, equalize lengths
- **Layout tools** — 6 alignments, distribute, fixed gap, match width/height, groups (Ctrl+G)
- **13 shapes** — process, decision, data, hexagon, chevron, star, document, cloud and more
- **Sketch recognition** — rough strokes become standard shapes; switch instantly if the guess is wrong
- **Image library** — upload PNG/JPG/SVG and reuse them like shapes, background removal, grayscale
- **Journal-style presets** — Nature / Cell / Mono, Okabe–Ito-based palettes plus your own colors
- **8 templates** — for papers (experimental workflow, CONSORT, PRISMA 2020, study design, ML pipeline) and reports (approval process, phase-gate, data flow)
- **Embed API** — put the editor into another website with an iframe and exchange documents through `postMessage` (see `embed-example.html`, `DEVELOPER.md`)

### Run locally

You can open the file by double-clicking it, but a simple static server is recommended for reliable fonts and image uploads.

```bash
python -m http.server 5173
```

Then open `http://localhost:5173` in your browser.

### Deploy to the web

Only static files are needed, so any of the following works for free.

1. **GitHub Pages** — in this repository, **Settings → Pages → Build and deployment**: Source = *Deploy from a branch*, Branch = *main* / *(root)* → Save. After 1–2 minutes the site is at `https://<account>.github.io/Workflow-assist/`.
2. **Netlify Drop** — drag this folder onto https://app.netlify.com/drop and you get a `https://<name>.netlify.app` address right away.
3. **Cloudflare Pages / Vercel** — upload the folder or connect this repository (no build command, output directory `/`).

All three let you connect a custom domain (add a CNAME record); HTTPS is applied automatically.

### Files

```
index.html           The whole app (HTML + CSS + JS, no dependencies) — this file is the source code
manual.html          Opens the user guide in the browser's language
manual_en.html       User guide — English
manual_kr.html       User guide — Korean
manual_jp.html       User guide — Japanese
manual_cn.html       User guide — Chinese (Simplified)
embed-example.html   Example of embedding the editor in another site via iframe + API
DEVELOPER.md         Developer documentation: integration, code map, JSON format, API (Korean)
README.md            This document
LICENSE              MIT
```

There is no build step: open `index.html` and it runs; put it on static hosting and it is live. No React/Vite tooling is needed — see `DEVELOPER.md` for adding it to an existing site.

### Browser support

Latest Chrome / Edge / Safari / Firefox. Auto-saved work, custom colors and the image library are stored per browser (use JSON export to move work to another computer).

### License

MIT — see `LICENSE`.

---

## 한국어

논문·보고서용 워크플로우 / 플로우차트를 **규격(그리드·간격·대칭 연결선)은 자동으로 지키되, 모든 요소는 자유롭게 조정**할 수 있게 그리는 웹 에디터입니다. 서버나 빌드 과정 없이 `index.html` 한 파일로 동작합니다.

- **바로 사용하기: [https://rundope.github.io/Workflow-assist/](https://rundope.github.io/Workflow-assist/)** (`index.html`)
- 사용 설명서: [`manual.html`](https://rundope.github.io/Workflow-assist/manual.html) (브라우저 언어에 맞는 설명서가 열립니다 — [English](https://rundope.github.io/Workflow-assist/manual_en.html) · [한국어](https://rundope.github.io/Workflow-assist/manual_kr.html) · [日本語](https://rundope.github.io/Workflow-assist/manual_jp.html) · [中文](https://rundope.github.io/Workflow-assist/manual_cn.html))
- 작업은 브라우저(localStorage)에 자동 저장되고, SVG / PNG / JSON으로 내보낼 수 있습니다.
- 화면 언어: 한국어 / English — 상단 바의 지구본 버튼으로 바꿉니다.

### 주요 기능

- **규격 자동 유지** — 그리드 스냅, 표준 간격, 대칭 직각 연결선, 이동 시 중심선 자석 가이드
- **자유로운 조정** — 위치·크기·글꼴·색을 드래그 또는 수치 입력으로 변경
- **연결선** — 포트 드래그로 생성(빈 곳에 놓으면 정렬된 새 노드 자동 생성), 끝점 재연결, 꺾임점 이동, 길이 통일
- **정렬 도구** — 정렬 6종, 균등 분배, 간격 고정, 너비·높이 길이 맞춤, 그룹(Ctrl+G)
- **도형 13종** — 프로세스·판단·데이터·육각형·화살 블록·별·문서·클라우드 등
- **손그림 인식** — 대충 그린 도형·선을 규격 도형으로 변환하고, 틀리면 즉시 다른 도형으로 전환
- **이미지 라이브러리** — PNG/JPG/SVG 업로드 후 도형처럼 재사용, 배경 제거(누끼), 회색조 변환
- **학술지 스타일 프리셋** — Nature / Cell / Mono, Okabe–Ito 계열 팔레트 + 사용자 색 추가
- **예제 8종** — 논문용(실험 워크플로우, CONSORT, PRISMA 2020, 연구 설계, ML 파이프라인), 보고서용(승인 프로세스, Phase-gate, 데이터 흐름도)
- **임베드 API** — 다른 웹사이트에 iframe으로 넣고 `postMessage`로 문서를 주고받기 (`embed-example.html`, `DEVELOPER.md` 참고)

### 로컬에서 실행

파일을 더블클릭해 열어도 되지만, 한글 폰트와 이미지 업로드를 안정적으로 쓰려면 간단한 정적 서버로 여는 것을 권장합니다.

```bash
python -m http.server 5173
```

브라우저에서 `http://localhost:5173` 을 엽니다.

### 웹에 올리기 (배포)

정적 파일만 있으면 되므로 아래 어느 방법이든 무료로 가능합니다.

1. **GitHub Pages** — 이 저장소의 **Settings → Pages → Build and deployment**: Source = *Deploy from a branch*, Branch = *main* / *(root)* → Save. 1~2분 뒤 `https://<계정>.github.io/Workflow-assist/` 로 접속됩니다.
2. **Netlify Drop** — https://app.netlify.com/drop 에 이 폴더를 끌어다 놓으면 바로 `https://<이름>.netlify.app` 주소가 생깁니다.
3. **Cloudflare Pages / Vercel** — 폴더를 업로드하거나 이 저장소를 연결합니다(빌드 명령 없음, 출력 디렉터리 `/`).

세 서비스 모두 사용자 지정 도메인을 연결할 수 있고(DNS에 CNAME 추가), HTTPS는 자동으로 적용됩니다.

### 파일 구조

```
index.html           앱 전체 (HTML + CSS + JS, 외부 의존성 없음) — 이 파일이 곧 소스코드
manual.html          브라우저 언어에 맞는 사용 설명서로 이동
manual_en.html       사용 설명서 — 영어
manual_kr.html       사용 설명서 — 한국어
manual_jp.html       사용 설명서 — 일본어
manual_cn.html       사용 설명서 — 중국어(간체)
embed-example.html   다른 웹사이트에 iframe으로 넣고 API로 통신하는 예제
DEVELOPER.md         개발자 문서 (통합 방법 · 코드 구조 · JSON 형식 · API)
README.md            이 문서
LICENSE              MIT
```

빌드 과정이 없습니다. `index.html`을 열면 그대로 실행되고, 정적 호스팅에 올리면 그대로 서비스됩니다. React/Vite 같은 도구는 필요 없으며, 기존 사이트에 넣는 방법은 `DEVELOPER.md`를 보세요.

### 브라우저 지원

Chrome / Edge / Safari / Firefox 최신 버전. 자동 저장된 작업, 사용자 색, 이미지 라이브러리는 브라우저별로 따로 저장됩니다(다른 PC로 옮기려면 JSON 내보내기를 사용하세요).

### 라이선스

MIT — `LICENSE` 참고.

---

## 日本語

論文・レポート用のワークフロー図／フローチャートを、**規格（グリッド・間隔・対称なコネクタ）は自動で守りつつ、すべての要素を自由に調整**できるように描くウェブエディタです。サーバーやビルドは不要で、`index.html` 1 ファイルで動作します。

- **すぐに使う：[https://rundope.github.io/Workflow-assist/](https://rundope.github.io/Workflow-assist/)**（`index.html`）
- ユーザーガイド：[`manual.html`](https://rundope.github.io/Workflow-assist/manual.html)（ブラウザの言語に合ったガイドが開きます — [English](https://rundope.github.io/Workflow-assist/manual_en.html) · [한국어](https://rundope.github.io/Workflow-assist/manual_kr.html) · [日本語](https://rundope.github.io/Workflow-assist/manual_jp.html) · [中文](https://rundope.github.io/Workflow-assist/manual_cn.html)）
- 作業はブラウザ（localStorage）に自動保存され、SVG / PNG / JSON で書き出せます。

> **画面の言語：** 英語と韓国語に対応しています（上部バーの地球アイコンで切り替え）。日本語の画面はまだないため、ユーザーガイドでは各機能名の横に英語と韓国語の表記を添えています。

### 主な機能

- **規格の自動維持** — グリッドスナップ、標準間隔、対称な直角コネクタ、移動時の中心線マグネットガイド
- **自由な調整** — 位置・サイズ・フォント・色をドラッグまたは数値入力で変更
- **コネクタ** — ポートのドラッグで作成（何もない場所に置くと整列した新しいノードを自動作成）、端点の再接続、折れ点の移動、長さの統一
- **整列ツール** — 整列 6 種、均等配置、間隔の固定、幅・高さを揃える、グループ（Ctrl+G）
- **図形 13 種** — 処理・判断・データ・六角形・矢印ブロック・星・文書・クラウドなど
- **手描き認識** — ざっと描いた図形や線を規格の図形に変換し、間違っていればすぐ別の図形に切り替え
- **画像ライブラリ** — PNG/JPG/SVG をアップロードして図形のように再利用、背景の除去、グレースケール変換
- **学術誌スタイルのプリセット** — Nature / Cell / Mono、Okabe–Ito 系のパレット＋ユーザー色の追加
- **テンプレート 8 種** — 論文用（実験ワークフロー、CONSORT、PRISMA 2020、研究デザイン、ML パイプライン）、レポート用（承認プロセス、Phase-gate、データフロー図）
- **埋め込み API** — 他のウェブサイトに iframe で埋め込み、`postMessage` で文書をやり取り（`embed-example.html`、`DEVELOPER.md` 参照）

### ローカルで実行

ファイルをダブルクリックして開くこともできますが、フォントや画像アップロードを安定して使うには、簡単な静的サーバーで開くことをおすすめします。

```bash
python -m http.server 5173
```

ブラウザで `http://localhost:5173` を開きます。

### ウェブに公開する

静的ファイルだけで動くので、次のどの方法でも無料で公開できます。

1. **GitHub Pages** — このリポジトリの **Settings → Pages → Build and deployment**：Source = *Deploy from a branch*、Branch = *main* / *(root)* → Save。1〜2 分後に `https://<アカウント>.github.io/Workflow-assist/` で開けます。
2. **Netlify Drop** — https://app.netlify.com/drop にこのフォルダをドラッグすると、すぐに `https://<名前>.netlify.app` のアドレスができます。
3. **Cloudflare Pages / Vercel** — フォルダをアップロードするか、このリポジトリを接続します（ビルドコマンドなし、出力ディレクトリ `/`）。

3 つとも独自ドメインを接続でき（DNS に CNAME を追加）、HTTPS は自動で適用されます。

### ファイル構成

```
index.html           アプリ全体（HTML + CSS + JS、外部依存なし）— このファイルがソースコード
manual.html          ブラウザの言語に合ったユーザーガイドへ移動
manual_en.html       ユーザーガイド — 英語
manual_kr.html       ユーザーガイド — 韓国語
manual_jp.html       ユーザーガイド — 日本語
manual_cn.html       ユーザーガイド — 中国語（簡体字）
embed-example.html   他のウェブサイトに iframe で埋め込み、API で通信するサンプル
DEVELOPER.md         開発者向けドキュメント（統合方法・コード構成・JSON 形式・API、韓国語）
README.md            この文書
LICENSE              MIT
```

ビルドは不要です。`index.html` を開けばそのまま動き、静的ホスティングに置けばそのまま公開されます。React/Vite などのツールは不要で、既存のサイトに組み込む方法は `DEVELOPER.md` を参照してください。

### 対応ブラウザ

Chrome / Edge / Safari / Firefox の最新版。自動保存された作業、ユーザー色、画像ライブラリはブラウザごとに別々に保存されます（他の PC に移すには JSON の書き出しを使ってください）。

### ライセンス

MIT — `LICENSE` を参照。

---

## 中文

一款用于绘制论文和报告用工作流程图 / 流程图的网页编辑器：**自动保持规范（网格、间距、对称连接线），同时每个元素都可以自由调整。** 无需服务器或构建，仅凭一个 `index.html` 文件即可运行。

- **在线使用：[https://rundope.github.io/Workflow-assist/](https://rundope.github.io/Workflow-assist/)**（`index.html`）
- 使用指南：[`manual.html`](https://rundope.github.io/Workflow-assist/manual.html)（按浏览器语言打开对应的指南 — [English](https://rundope.github.io/Workflow-assist/manual_en.html) · [한국어](https://rundope.github.io/Workflow-assist/manual_kr.html) · [日本語](https://rundope.github.io/Workflow-assist/manual_jp.html) · [中文](https://rundope.github.io/Workflow-assist/manual_cn.html)）
- 作品会自动保存在浏览器（localStorage）中，并可导出为 SVG / PNG / JSON。

> **界面语言：** 支持英语和韩语（用顶部栏的地球图标切换）。暂无中文界面，因此使用指南在每个功能名称旁标注了英语和韩语原文。

### 主要功能

- **自动保持规范** — 网格吸附、标准间距、对称直角连接线、拖动时的中心线磁性参考线
- **自由调整** — 通过拖动或输入数值更改位置、大小、字体和颜色
- **连接线** — 拖动端口即可创建（放到空白处会自动生成对齐的新节点），可重新连接端点、移动折弯点、统一长度
- **对齐工具** — 6 种对齐、均匀分布、固定间距、统一宽度/高度、编组（Ctrl+G）
- **13 种图形** — 处理、判断、数据、六边形、箭头块、星形、文档、云形等
- **手绘识别** — 把随手画的图形和线条转换为规范图形，识别错误时可立即切换
- **图片库** — 上传 PNG/JPG/SVG 后像图形一样重复使用，支持去除背景和灰度转换
- **期刊风格预设** — Nature / Cell / Mono，基于 Okabe–Ito 的调色板，并可添加自定义颜色
- **8 个示例模板** — 论文用（实验工作流程、CONSORT、PRISMA 2020、研究设计、ML 流程）和报告用（审批流程、Phase-gate、数据流图）
- **嵌入 API** — 用 iframe 嵌入其他网站，通过 `postMessage` 交换文档（参见 `embed-example.html`、`DEVELOPER.md`）

### 本地运行

双击文件即可打开，但为了稳定使用字体和图片上传，建议用简单的静态服务器打开。

```bash
python -m http.server 5173
```

然后在浏览器中打开 `http://localhost:5173`。

### 发布到网上

只需要静态文件，因此以下任何一种方式都可以免费发布。

1. **GitHub Pages** — 在本仓库的 **Settings → Pages → Build and deployment** 中：Source = *Deploy from a branch*，Branch = *main* / *(root)* → Save。1~2 分钟后即可通过 `https://<账号>.github.io/Workflow-assist/` 访问。
2. **Netlify Drop** — 把此文件夹拖到 https://app.netlify.com/drop ，立即获得 `https://<名称>.netlify.app` 地址。
3. **Cloudflare Pages / Vercel** — 上传文件夹或连接本仓库（无构建命令，输出目录 `/`）。

三者都可以绑定自定义域名（在 DNS 中添加 CNAME），并自动启用 HTTPS。

### 文件结构

```
index.html           整个应用（HTML + CSS + JS，无外部依赖）— 此文件即源代码
manual.html          按浏览器语言跳转到对应的使用指南
manual_en.html       使用指南 — 英语
manual_kr.html       使用指南 — 韩语
manual_jp.html       使用指南 — 日语
manual_cn.html       使用指南 — 中文（简体）
embed-example.html   用 iframe 嵌入其他网站并通过 API 通信的示例
DEVELOPER.md         开发者文档（集成方法、代码结构、JSON 格式、API，韩语）
README.md            本文档
LICENSE              MIT
```

无需构建：打开 `index.html` 即可运行，放到静态托管上即可上线。不需要 React/Vite 等工具；如需集成到现有网站，请参阅 `DEVELOPER.md`。

### 浏览器支持

最新版 Chrome / Edge / Safari / Firefox。自动保存的作品、自定义颜色和图片库按浏览器分别保存（如需转移到其他电脑，请使用 JSON 导出）。

### 许可证

MIT — 参见 `LICENSE`。
