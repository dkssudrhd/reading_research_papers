# 논문 추가 규칙 (LLM용 작업 지침)

이 문서는 이 저장소에 새 논문 노트를 추가할 때 **어떤 LLM이 읽어도 바로 파일을 생성할 수 있도록** 만든 자기완결적 규격서다. 사람은 아래 "필요한 입력"만 채워서 지시하면 되고, 실행하는 LLM은 이 문서만으로 4개 파일(papers/, translations/, papers.json, index.html)을 모두 만들거나 갱신할 수 있어야 한다.

이 저장소는 빌드 도구가 없는 순수 정적 HTML 사이트다. React/Vue 같은 프레임워크나 템플릿 엔진을 쓰지 않는다. 모든 페이지는 `assets/style.css` 하나만 공유하며, 페이지 안에 `<style>`을 새로 넣지 않는다.

---

## 0. 필요한 입력 (사람이 채우는 것)

새 논문을 추가할 때 최소한 아래 정보가 있어야 한다. 없으면 LLM이 원문(arXiv 등)에서 직접 찾아야 한다.

| 항목 | 설명 | 필수 |
|---|---|---|
| 논문 제목 (원문) | 영문 원제 | 필수 |
| 원문 URL | arXiv abstract 페이지 또는 학회/저널 페이지 | 필수 |
| 날짜 | `YYYY-MM-DD` (보통 오늘 또는 지정한 평일) | 필수 |
| 카테고리 | 아래 5개 중 하나 (모르면 LLM이 태그로 추론) | 선택 |
| 난이도 / 실무 관련도 | 1~5점, 없으면 LLM이 본문 판단으로 채움 | 선택 |

---

## 1. 파일 및 이름 규칙

- **slug**: 논문 제목을 kebab-case 영문 슬러그로 만든다. 예: `zero-shot-multi-animal-tracking`
- **papers/YYYY-MM-DD-{slug}.html** — 항상 생성. 요약 노트.
- **translations/YYYY-MM-DD-{slug}-full-ko.html** — 라이선스가 명확히 허용될 때만 생성 (전문 번역, 아래 3절 참고).
- **translations/YYYY-MM-DD-{slug}-ko.html** — 라이선스가 불명확하지만 그래도 한국어로 깊이 있게 정리하고 싶을 때 쓰는 **레거시 형식**. 새 항목에는 기본적으로 만들지 않아도 되며, 대신 전문 번역을 보류하고 요약 노트(papers/)만 충실히 쓰는 쪽을 우선한다. 굳이 만든다면 4절의 "읽기본" 템플릿을 따른다.
- 모든 HTML은 `<head>`에 `<link rel="stylesheet" href="../assets/style.css">` 한 줄만 두고, 새 `<style>` 블록을 추가하지 않는다.

---

## 2. 라이선스 확인 규칙 (전문 번역 여부를 결정)

전문 번역(`-full-ko.html`)을 공개 저장소에 커밋하려면 아래를 **반드시** 확인한다.

1. **논문 자체 라이선스**: arXiv 페이지 하단/사이드의 라이선스 링크를 확인한다.
   - `CC BY 4.0`, `CC BY-SA 4.0` 등 번역·파생 저작물 작성을 명시적으로 허용하는 라이선스 → **전문 번역 가능**.
   - arXiv의 기본 `arXiv.org perpetual, non-exclusive license` **만** 있고 별도 CC 라이선스가 없음 → **전문 번역 불가** (배포 허용 범위가 번역·파생 저작물까지 포함하는지 불명확하기 때문).
   - 학회/저널(IEEE, MDPI 등) 게재본이라면 해당 출판사 라이선스도 함께 확인한다.
2. **코드 저장소 라이선스** (참고용, 논문 본문 번역 가능 여부와는 별개): MIT/Apache-2.0 등 확인되면 기록만 한다.
3. **데이터셋 라이선스**: 공식 페이지에서 확인되면 기록하고, 안 보이면 "찾지 못함"이라고 명시한다.

**결정 규칙**:
- 1번이 CC BY/CC BY-SA 계열로 확인됨 → `translations/...-full-ko.html` 생성, `translation_status: "completed"`.
- 1번이 불명확 → 전문 번역 생성하지 않음, `translation_status: "skipped_due_to_license"`, `papers.json`의 `full_translation_html`은 빈 문자열 `""`, index.html 해당 칸에는 `라이선스 불명확으로 보류` 텍스트만 넣는다 (링크 없음).
- 어느 경우든 `license_note` 필드에 위 세 가지(논문/코드/데이터셋)에 대해 **무엇을 확인했고 무엇을 못 찾았는지**를 한국어 문장으로 구체적으로 적는다. "라이선스 확인함" 같은 뭉뚱그린 문장 금지.
- 요약 노트(`papers/*.html`)는 원문을 그대로 옮기는 것이 아니라 재구성한 2차 저작물이므로 라이선스와 무관하게 항상 작성한다.

---

## 3. 카테고리 (5종 고정)

| category (papers.json) | 배지 클래스 | 필터 data-filter | 한국어 라벨 |
|---|---|---|---|
| `Small Object Detection` | `cat-sod` | `sod` | 작은 객체 탐지 |
| `Multi-Object Tracking` | `cat-mot` | `mot` | 다중 객체 추적 |
| `PTZ Camera Tracking` | `cat-ptz` | `ptz` | PTZ 카메라 추적 |
| `Wind Farm Bird Monitoring` | `cat-wind` | `wind` | 풍력단지 모니터링 |
| `Real-time Computer Vision` | `cat-rt` | `rt` | 실시간 컴퓨터 비전 |

새 카테고리를 추가하지 않는다. 애매하면 논문의 핵심 기여(주요 실험이 무엇을 개선하는가)를 기준으로 5개 중 하나를 고른다.

---

## 4. `papers/YYYY-MM-DD-{slug}.html` 템플릿 (요약 노트, 항상 생성)

```html
<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>{{한글 요약 제목}} 연구 노트</title>
  <link rel="stylesheet" href="../assets/style.css">
</head>
<body>
  <main class="page">
    <section class="hero">
      <p class="eyebrow">Daily Paper Reading Note</p>
      <h1>{{한글 요약 제목}}</h1>
      <p class="lede">{{2~3문장 소개. 왜 이 논문을 골랐는지, 조류충돌방지 시스템 관점에서 무엇을 얻는지}}</p>

      <!-- 전문 번역을 만들었다면만 포함 -->
      <div class="translation-link-box">
        <strong>한국어 전문 번역</strong>
        <p>원문 구조를 따라 읽는 별도 페이지가 있습니다. 요약 노트와 전문 번역은 다른 파일입니다.</p>
        <div class="quick-links">
          <a href="../translations/{{slug}}-full-ko.html">전문 번역 열기</a>
          <a href="{{원문 URL}}" target="_blank" rel="noreferrer">원문 arXiv</a>
        </div>
      </div>

      <div class="meta-grid" aria-label="논문 메타데이터">
        <div class="meta"><strong>날짜</strong>{{YYYY-MM-DD}}</div>
        <div class="meta"><strong>연도 / venue</strong>{{연도}} / {{venue}}</div>
        <div class="meta"><strong>난이도 / 실무성</strong>{{n}} / 5, {{n}} / 5</div>
        <div class="meta"><strong>라이선스</strong>{{확인 결과 한 줄}}</div>
        <div class="meta"><strong>코드</strong><a href="{{code}}" target="_blank" rel="noreferrer">GitHub 공개 저장소</a></div>
        <div class="meta"><strong>데이터셋</strong>{{dataset 또는 "찾지 못함"}}</div>
        <div class="meta"><strong>핵심 키워드</strong>{{쉼표로 나열}}</div>
        <div class="meta"><strong>원문</strong><a href="{{원문 URL}}" target="_blank" rel="noreferrer">arXiv abstract</a></div>
      </div>

      <div class="tag-list" aria-label="주제 태그">
        <span class="tag">{{tag1}}</span><span class="tag">{{tag2}}</span><span class="tag">{{tag3}}</span><span class="tag">{{tag4}}</span><span class="tag">{{tag5}}</span>
      </div>
    </section>

    <section id="summary">
      <h2>요약</h2>
      <div class="summary-box">{{핵심 주장 2~3문장}}</div>
      <div class="cards">
        <div class="card"><span class="mini-label">한 줄 판단</span>{{실무 적용 가능성 한 줄}}</div>
        <div class="card"><span class="mini-label">왜 중요하나</span>{{차별점 한 줄}}</div>
        <div class="card"><span class="mini-label">현장 연결점</span>{{조류충돌방지 시스템과의 연결 한 줄}}</div>
      </div>
    </section>

    <section id="why">
      <h2>왜 이 논문인가</h2>
      <div class="grid-2">
        <div class="practice-box"><h3>직접 관련성</h3><p>{{오늘 주제와의 연결}}</p></div>
        <div class="warning-box"><h3>주의할 점</h3><p>{{한계, 도메인 갭 등}}</p></div>
      </div>
      <div class="cards">
        <div class="card"><h3>코드 확인됨/안됨</h3><p>{{...}}</p></div>
        <div class="card"><h3>라이선스 확인됨/안됨</h3><p>{{...}}</p></div>
        <div class="card"><h3>실무성 평가</h3><p>{{...}}</p></div>
      </div>
    </section>

    <section id="core-idea">
      <h2>핵심 아이디어</h2>
      <div class="grid-2">
        <div class="card"><h3>핵심 문장</h3><p>{{방법론 핵심 1~2문단}}</p></div>
        <div class="figure" aria-label="{{설명}}">
          <svg viewBox="0 0 940 260" role="img" aria-label="{{파이프라인 설명}}">
            <!-- 박스 3~4개 + 화살표로 된 단순 흐름도. 기존 파일들의 svg 구조를 참고해 새로 그린다 -->
          </svg>
          <div class="caption">{{원문 Figure 번호 기반 설명}}</div>
        </div>
      </div>
    </section>

    <section id="figures">
      <h2>그림과 표를 어떻게 읽을까</h2>
      <table>
        <thead><tr><th>원문 번호</th><th>뜻</th><th>실무 포인트</th></tr></thead>
        <tbody>
          <tr><td>Figure N</td><td>{{설명}}</td><td>{{조류충돌방지 관점 메모}}</td></tr>
        </tbody>
      </table>
    </section>

    <section id="technical-details">
      <h2>기술 핵심</h2>
      <div class="grid-2">
        <div class="card"><span class="mini-label">{{항목명}}</span><div class="metric">{{숫자/핵심값}}</div><p>{{설명}}</p></div>
        <!-- 3~4개 반복 -->
      </div>
    </section>

    <section id="application">
      <h2>조류 충돌 방지 시스템에 어떻게 쓰나</h2>
      <div class="cards">
        <div class="card"><h3>조류 추적</h3><p>{{...}}</p></div>
        <div class="card"><h3>PTZ 카메라 추적</h3><p>{{...}}</p></div>
        <div class="card"><h3>YOLO 데이터/학습</h3><p>{{...}}</p></div>
        <div class="card"><h3>실시간 서버 운영</h3><p>{{...}}</p></div>
        <div class="card"><h3>Hanwha Vision / RTSP / PTZ</h3><p>{{...}}</p></div>
        <!-- 논문 성격에 맞는 5~6개 -->
      </div>
    </section>

    <section id="experiment">
      <h2>바로 해볼 실험 3가지</h2>
      <div class="grid-3">
        <div class="card"><h3>1. {{실험명}}</h3><p>{{방법}}</p><p><strong>측정:</strong> {{지표}}</p></div>
        <!-- 총 3개 -->
      </div>
    </section>

    <section id="terms">
      <h2>용어 정리</h2>
      <table>
        <thead><tr><th>용어</th><th>뜻</th></tr></thead>
        <tbody><tr><td>{{용어}}</td><td>{{설명}}</td></tr></tbody>
      </table>
    </section>

    <section id="links">
      <h2>링크</h2>
      <ul>
        <li><a href="{{arXiv abstract}}" target="_blank" rel="noreferrer">원문 arXiv 초록 페이지</a></li>
        <li><a href="{{arXiv pdf}}" target="_blank" rel="noreferrer">원문 PDF</a></li>
        <li><a href="{{code}}" target="_blank" rel="noreferrer">공식 GitHub 코드 저장소</a></li>
        <!-- 전문 번역이 있다면 -->
        <li><a href="../translations/{{slug}}-full-ko.html">한국어 전문 번역 페이지</a></li>
      </ul>
    </section>

    <section id="checklist">
      <h2>최종 체크리스트</h2>
      <ul class="checklist">
        <li>{{점검 문장}}</li>
        <!-- 5~6개 -->
      </ul>
      <!-- 전문 번역이 있다면 -->
      <div class="translation-link-box" style="margin-top:18px;">
        <strong>한국어 전문 번역으로 이동</strong>
        <div class="quick-links"><a href="../translations/{{slug}}-full-ko.html">전문 번역 열기</a></div>
      </div>
    </section>
  </main>
</body>
</html>
```

섹션 순서(`summary` → `why` → `core-idea` → `figures` → `technical-details` → `application` → `experiment` → `terms` → `links` → `checklist`)는 기존 9개 노트 전부와 동일하다. 순서를 바꾸지 않는다.

---

## 5. `translations/YYYY-MM-DD-{slug}-full-ko.html` 템플릿 (라이선스 확인됨)

전문 번역은 원문 구조(초록 → 절 순서 → 결론)를 그대로 따라가는 별도 페이지다.

```html
<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>{{한글 제목}} 한국어 전문 번역</title>
  <link rel="stylesheet" href="../assets/style.css">
</head>
<body>
  <main class="page">
    <a class="back-link" href="../papers/{{slug}}.html">← 요약 노트로 돌아가기</a>
    <section class="hero">
      <p class="eyebrow">Korean Full Translation</p>
      <h1>{{한글 제목}}</h1>
      <p class="lede">이 문서는 원문 라이선스가 번역과 파생 작성이 가능한 것으로 확인된 논문의 한국어 전문 번역본입니다. 원문 링크와 라이선스, 변경 고지를 함께 확인하고 읽으세요.</p>

      <div class="notice" id="translation-notice">
        <strong>번역 고지</strong>
        <p>{{어떤 라이선스를 근거로 번역했는지 한 문단}}</p>
        <p>원문 링크: <a href="{{arXiv abstract}}" target="_blank" rel="noreferrer">arXiv abstract</a> / <a href="{{arXiv pdf}}" target="_blank" rel="noreferrer">PDF</a></p>
      </div>

      <div class="meta-grid" id="paper-info" aria-label="논문 메타데이터">
        <div class="meta"><strong>Original title</strong>{{영문 원제}}</div>
        <div class="meta"><strong>Korean title</strong>{{한글 제목}}</div>
        <div class="meta"><strong>Authors</strong>{{저자}}</div>
        <div class="meta"><strong>Year / Venue</strong>{{연도}} / {{venue}}</div>
        <div class="meta"><strong>Original paper URL</strong><a href="{{...}}" target="_blank" rel="noreferrer">{{...}}</a></div>
        <div class="meta"><strong>PDF URL</strong><a href="{{...}}" target="_blank" rel="noreferrer">{{...}}</a></div>
        <div class="meta"><strong>Code URL</strong><a href="{{...}}" target="_blank" rel="noreferrer">{{...}}</a></div>
        <div class="meta"><strong>Dataset URL</strong>{{... 또는 "찾지 못함"}}</div>
      </div>

      <div class="warning">
        <strong>저작권 메모</strong>
        <p>{{라이선스 근거를 다시 한 문장으로}}</p>
      </div>
    </section>

    <section id="full-abstract">
      <h2>초록 전문 번역</h2>
      <div class="translation-block"><p>{{초록 문단별 번역}}</p></div>
    </section>

    <section id="full-translation">
      <h2>논문 본문 전문 번역</h2>
      <article class="translation-block">
        <h3>1. {{절 제목}}</h3>
        <p>{{번역 문단}}</p>
      </article>
      <!-- 원문의 절 수만큼 article.translation-block 반복. 하위 항목은 h4 사용 -->
    </section>

    <section id="equations-and-terms">
      <h2>수식과 용어</h2>
      <div class="equation"><code>{{수식}}</code></div>
      <p>{{수식 설명}}</p>
      <table>
        <thead><tr><th>용어</th><th>설명</th></tr></thead>
        <tbody><tr><td>{{용어}}</td><td>{{설명}}</td></tr></tbody>
      </table>
    </section>

    <section id="figures-and-tables">
      <h2>그림과 표</h2>
      <div class="figure">
        <svg viewBox="0 0 980 220" role="img" aria-label="{{설명}}"><!-- 개념도 --></svg>
        <div class="caption">{{설명}}</div>
      </div>
      <table>
        <thead><tr><th>원문 번호</th><th>한국어 설명</th><th>시스템 적용 관점</th></tr></thead>
        <tbody><tr><td>Figure N</td><td>{{...}}</td><td>{{...}}</td></tr></tbody>
      </table>
    </section>

    <section id="translator-notes">
      <h2>번역자 메모</h2>
      <div class="translation-block">
        <ul><li><strong>{{용어}}</strong>는 {{번역어}}로 번역했다. {{이유}}</li></ul>
      </div>
    </section>

    <section id="practical-reading-notes">
      <h2>실무 읽기 노트</h2>
      <div class="grid-3">
        <div class="practical"><h3>bird detection</h3><p>{{...}}</p></div>
        <!-- PTZ camera tracking / RTSP·edge·server / YOLO·training data / ONNX·TensorRT·TFLite / Hanwha Vision 연동 등 5~6개 -->
      </div>
    </section>

    <section id="original-links">
      <h2>원문 링크</h2>
      <ul>
        <li><a href="{{arXiv abstract}}" target="_blank" rel="noreferrer">arXiv 초록 페이지</a></li>
        <li><a href="{{arXiv pdf}}" target="_blank" rel="noreferrer">arXiv PDF</a></li>
        <li><a href="{{code}}" target="_blank" rel="noreferrer">공식 GitHub 저장소</a></li>
        <li><a href="../papers/{{slug}}.html">요약 노트</a></li>
      </ul>
    </section>

    <section id="reading-checklist">
      <h2>읽기 체크리스트</h2>
      <ul class="checklist"><li>{{점검 문장}}</li></ul>
      <div class="warning"><strong>실무 주의</strong><p>{{도메인 갭 경고}}</p></div>
    </section>
  </main>
</body>
</html>
```

---

## 6. `translations/YYYY-MM-DD-{slug}-ko.html` 템플릿 (레거시 읽기본, 라이선스 불명확할 때만 선택적으로)

원문을 그대로 옮기지 않고 초록만 번역 + 나머지는 절별 상세 의역/요약으로 재구성한다. 섹션 순서: `translation-notice` → `paper-info`(표) → `abstract-ko` → `section-by-section`(카드) → `figure-table-guide`(표) → `important-terms`(카드) → `practical-notes` → `original-links` → `reading-checklist`(`<ol>`). 상세 구조는 `translations/2026-07-07-sahi-small-object-detection-ko.html`을 참고한다 (헤더는 `<header class="hero">`, 상단에 `<nav class="quick-links">` 앵커 목록 포함).

이 형식은 기본값이 아니다. 라이선스가 불명확하면 **먼저 이 레거시 읽기본을 만들지, 아니면 번역 없이 요약 노트만 둘지**를 판단한다. 최근 항목들은 후자(번역 보류)를 기본으로 택했다.

---

## 7. `papers.json`에 항목 추가

배열의 **맨 앞**에 아래 형태로 객체를 하나 추가한다 (최신 항목이 배열 첫 번째).

```json
{
  "date": "YYYY-MM-DD",
  "title": "{{영문 원제}}",
  "year": "{{연도}}",
  "venue": "{{학회/저널/arXiv preprint}}",
  "url": "{{arXiv abstract URL}}",
  "pdf_url": "{{arXiv pdf URL}}",
  "code": "{{GitHub URL 또는 '찾지 못함'}}",
  "dataset": "{{URL 또는 '찾지 못함'}}",
  "tags": ["tag1", "tag2", "tag3", "tag4", "tag5"],
  "category": "{{5종 중 하나, 정확한 영문 표기}}",
  "difficulty": 1,
  "practicality": 1,
  "html": "papers/YYYY-MM-DD-{slug}.html",
  "full_translation_html": "translations/YYYY-MM-DD-{slug}-full-ko.html 또는 \"\"",
  "translation_type": "original_paper_full_korean_translation",
  "translation_status": "completed 또는 skipped_due_to_license",
  "translation_scope": "{{무엇을 번역했는지/왜 보류했는지 한국어 설명}}",
  "license_note": "{{논문/코드/데이터셋 라이선스 확인 결과를 각각 구체적으로}}",
  "one_line_summary": "{{한국어 한 문장 요약}}"
}
```

`difficulty`, `practicality`는 정수 1~5. 필드 이름과 순서는 기존 9개 항목과 동일하게 맞춘다 (JSON 파서는 순서에 상관없지만, 사람이 diff를 보기 쉽도록 통일한다).

---

## 8. `index.html`에 행 추가

`<tbody id="paperRows">`의 **맨 위**(첫 번째 `<tr>` 앞)에 아래 행을 추가한다. 열 순서는 날짜 / 분류 / 논문 제목 / 주제 태그 / 난이도 / 실무 관련도 / 핵심 키워드 / 요약 HTML / 한국어 읽기본 / 한국어 전문 번역, 총 10칸이다.

```html
<tr data-category="{{sod|mot|ptz|wind|rt}}">
  <td>YYYY-MM-DD</td>
  <td><span class="category-badge cat-{{sod|mot|ptz|wind|rt}}">{{한국어 카테고리 라벨}}</span></td>
  <td>{{영문 원제}}</td>
  <td><div class="tag-list"><span class="tag">{{tag1}}</span><span class="tag">{{tag2}}</span><span class="tag">{{tag3}}</span></div></td>
  <td>{{난이도}} / 5</td>
  <td><span class="score">{{실무관련도}} / 5</span></td>
  <td>{{핵심 키워드 쉼표 나열}}</td>
  <td><a href="papers/YYYY-MM-DD-{slug}.html">노트 열기</a></td>
  <td>별도 읽기본 없음</td>
  <!-- 전문 번역이 있으면: -->
  <td><a href="translations/YYYY-MM-DD-{slug}-full-ko.html">전문 번역 열기</a></td>
  <!-- 없으면: -->
  <td>라이선스 불명확으로 보류</td>
</tr>
```

행을 추가하면 필터 탭(`.filter-btn`)과 개수 표시는 `index.html`의 인라인 `<script>`가 `#paperRows`의 `<tr>` 개수를 자동으로 세므로 **따로 손댈 필요가 없다**.

---

## 9. 디자인 시스템 — 새 클래스를 만들기 전에

모든 시각 스타일은 `assets/style.css`에 있다. 새 페이지를 만들 때 아래 기존 클래스만으로 대부분 표현할 수 있으니, 새 CSS를 추가하기 전에 먼저 이 목록에서 찾는다.

- **레이아웃**: `main.page`, `.hero`, `section`/`section.card`, `header.card`
- **텍스트**: `.eyebrow`, `.lede`/`.lead`, `.label`/`.mini-label`, `.small`/`.caption`/`.source-note`, `.metric`, `.score`
- **배지/링크**: `.pill`, `.tag`/`.tag-list`, `.quick-links`, `.back-link`/`.back`, `.top-link`/`.bottom-link`
- **그리드/카드**: `.grid`, `.grid-2`, `.grid-3`, `.cards`, `.meta-grid`+`.meta`, `.card`, `.figure`(+내부 `svg`+`.caption`)
- **콜아웃 박스**: `.notice`(파랑), `.warning`/`.warning-box`(빨강), `.practical`/`.practice-box`(초록), `.summary`/`.summary-box`, `.translator`(주황), `.translation-block`, `.translation-link-box`
- **플로우 다이어그램**: `.flow`+`.step`+`.arrow` (간단한 박스-화살표 나열), 또는 `.figure`에 직접 `<svg>` (기존 파일들의 svg 마크업 참고)
- **표**: 기본 `table`/`th`/`td`, index.html에서는 `.panel`+`.table-wrap`
- **체크리스트**: `ul.checklist`
- **수식**: `.equation`
- **벤치마크 막대그래프**: `.bar-table`/`.bar-row`/`.bar-label`/`.bar-track`/`.bar-fill`/`.bar-value`
- **index.html 전용**: `.category-badge.cat-*`, `.filters`/`.filter-btn`

정말로 기존 클래스로 표현이 안 되는 새로운 시각 요소가 필요하면, `assets/style.css`에 새 규칙을 추가하고 이 문서의 목록도 함께 갱신한다. 인라인 `style=""` 속성은 그림 강조 등 아주 사소한 경우 외에는 피한다.

---

## 10. 체크리스트 (작업 완료 전 최종 확인)

1. `papers/YYYY-MM-DD-{slug}.html` 생성, 섹션 10개(순서 고정) 모두 채움
2. 라이선스 확인 후 전문 번역 생성 여부 결정, 생성했다면 `translations/YYYY-MM-DD-{slug}-full-ko.html`
3. `papers.json` 배열 맨 앞에 항목 추가 (모든 필드 채움, `category` 포함)
4. `index.html`의 `#paperRows` 맨 위에 행 추가 (`data-category` + `.category-badge` 일치 확인)
5. 새 HTML 파일 모두 `<link rel="stylesheet" href="../assets/style.css">`만 사용, 새 `<style>` 블록 없음
6. `README.md`는 별도로 고칠 필요 없음 (카테고리/디자인 시스템 설명은 이미 반영됨)
