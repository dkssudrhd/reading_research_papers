# Daily Paper Reading Notes

조류충돌방지 시스템, 컴퓨터 비전, 작은 객체 탐지, PTZ 카메라 추적, 실시간 영상 분석 관련 논문을 평일마다 1편씩 읽고 정리하는 저장소입니다.

## 주요 주제

- Bird Detection
- Small Object Detection
- Multi-Object Tracking
- PTZ Camera Tracking
- Wind Farm Bird Monitoring
- Real-time Computer Vision

## 파일 구성

- `index.html`: 매일 읽은 논문 목록, 핵심 메타데이터, 주제별 필터 탭
- `papers/`: 날짜별 HTML 논문 요약 노트
- `translations/`: 저작권 범위에 맞춘 한국어 읽기본 또는 상세 의역본
- `papers.json`: 논문 목록을 재사용하기 위한 구조화 데이터 (`category` 필드 포함)
- `assets/style.css`: 모든 HTML 페이지가 공유하는 디자인 시스템 (색상 토큰, 카드/배지/표/체크리스트 등 공통 컴포넌트). 페이지별로 `<style>`을 따로 두지 않고 `<link rel="stylesheet" href="assets/style.css">`(또는 `../assets/style.css`)만 참조합니다.
- `vla-study-roadmap.html`: Vision-Language-Action(VLA)를 기초 개념부터 2026년 공개 연구 흐름까지 학습하는 논문 로드맵입니다.

## 분류(카테고리)

논문은 아래 6개 카테고리 중 하나로 분류하며, `index.html` 상단 필터 탭과 각 행의 배지, `papers.json`의 `category` 필드에 동일하게 반영합니다.

- Small Object Detection (`sod`)
- Multi-Object Tracking (`mot`)
- PTZ Camera Tracking (`ptz`)
- Wind Farm Bird Monitoring (`wind`)
- Real-time Computer Vision (`rt`)
- Vision-Language-Action (`vla`)

새 논문을 추가할 때는 `index.html` 표에 `<tr data-category="...">`와 `<span class="category-badge cat-...">분류명</span>`을 추가하고, `papers.json`에도 같은 `category` 값을 기록합니다.

## HTML로 보기

GitHub 저장소 화면에서는 HTML 파일이 코드로 보입니다. 렌더링된 웹페이지 형태로 보려면 GitHub Pages를 사용합니다.

1. GitHub 저장소에서 `Settings` -> `Pages`로 이동합니다.
2. `Build and deployment`의 `Source`를 `Deploy from a branch`로 선택합니다.
3. `Branch`를 `main`, 폴더를 `/ (root)`로 선택하고 저장합니다.
4. 잠시 뒤 아래 주소에서 논문 목록을 볼 수 있습니다.

```text
https://dkssudrhd.github.io/reading_research_papers/
```

## 운영 방식

평일마다 업무와 연구에 직접 도움이 되는 논문 1편을 선정하고, 논문 내용을 한국어로 쉽게 요약합니다. 각 노트는 요약 페이지와 한국어 읽기본으로 나뉘며, 읽기본은 라이선스 확인 범위에 맞춰 전문 번역 또는 상세 의역 방식으로 작성합니다.

## 새 논문 추가하기

새 논문을 추가하는 절차, HTML 템플릿, 라이선스 확인 규칙, `papers.json`/`index.html` 갱신 방법은 [CONTRIBUTING.md](CONTRIBUTING.md)에 정리되어 있습니다. 다른 LLM에게 논문 추가를 맡길 때는 이 문서를 함께 읽게 하면 됩니다.
