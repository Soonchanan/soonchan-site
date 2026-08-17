# 안순찬 개인 사이트

두 페이지짜리 정적 사이트. 빌드 도구 없음 — HTML/CSS/바닐라 JS만으로 동작하며, 파일을 그대로 아무 정적 호스팅(Vercel, Netlify, GitHub Pages)에 올리면 끝.

## 구조

```
/
├── index.html   → 홈. CV 스타일 자기소개 (학력·경력·수상·자격증·기타활동)
├── values.html  → 가치관 53개 원칙 (10부 구성)
└── README.md
```

두 페이지는 서로 우측 상단 내비게이션으로 연결돼 있음 (`index.html` ↔ `values.html`).

## 디자인 시스템

컨셉은 흑백 동양화 — 여백 많은 종이 위에 먹빛 잉크 마크 하나. "개별 항목들이 겹쳐서 하나의 그림을 이룬다"는 게 콘텐츠 자체의 주제라, 시각 언어도 그걸 따름 (`values.html`의 51번 항목 참고).

### 컬러 토큰 (모든 페이지 `:root`에 동일하게 정의)

| 변수 | 값 | 용도 |
|---|---|---|
| `--ink` | `#1c1a17` | 본문 텍스트, 짙은 배경 |
| `--paper` | `#f1ede4` | 배경 (종이 톤 — AI 느낌 나는 크림색 `#F4F1EA`보다 살짝 회색 쪽으로 틀어둔 값이니 바꾸지 말 것) |
| `--paper-deep` | `#e5ddcf` | 섹션 배경 구분용 |
| `--mist` | `#83796a` | 보조 텍스트 |
| `--mist-light` | `#a89f8f` | 더 옅은 보조 텍스트 (날짜 등) |
| `--seal` | `#9c2f22` | 포인트 컬러 — 도장(낙관) 색. 아주 아껴서 씀 (라벨, 잉크마크 점 하나 정도) |
| `--line` / `--line-strong` | `rgba(28,26,23,.14)` / `.28` | 구분선 |

### 타이포그래피

**Pretendard Variable 하나만 사용.** 다른 폰트 섞지 말 것 — 이전에 Song Myung/Noto Serif KR/IBM Plex Mono를 섞어 썼다가 "일괄 Pretendard로" 요청받고 통일함.

```html
<link rel="stylesheet" as="style" crossorigin
  href="https://cdn.jsdelivr.net/gh/orioncactus/[email protected]/dist/web/variable/pretendardvariable.css" />
```
```css
font-family:'Pretendard Variable', Pretendard, -apple-system, BlinkMacSystemFont, system-ui, Roboto, sans-serif;
```

위계는 폰트가 아니라 **굵기(weight)와 자간(letter-spacing)**으로 낸다:
- 헤드라인: `font-weight:800`, `letter-spacing:-0.02em`
- 섹션 타이틀: `font-weight:800`
- 아이템 타이틀: `font-weight:700`
- 본문: 기본 weight (400~500)
- 라벨/이유브로우/날짜: `font-weight:600~700`, `letter-spacing:0.12~0.18em`, 대문자, `font-variant-numeric: tabular-nums` (숫자 정렬용 — 모노스페이스 폰트 없이 정렬 맞추는 트릭)

### 시그니처 요소 — 잉크 마크

순수 CSS로 만든 도장(낙관) 모양. `radial-gradient` + `mix-blend-mode:multiply` + `filter:blur()`를 겹친 `<div class="ink-mark"><span></span>...</div>`. **절대 텍스트 뒤 중앙에 큰 블롭으로 깔지 말 것** — 처음 그렇게 했다가 가독성 깨져서, 지금처럼 헤더 모서리에 작게(180~300px) 찍는 형태로 고정함. 새 페이지 만들 때도 이 크기·위치 규칙 유지.

`prefers-reduced-motion: reduce`에서 breathe 애니메이션 끄는 처리 돼 있음 — 새 애니메이션 추가 시에도 이 패턴 유지.

### 구조적 장치

- `values.html`의 스크롤 진행바(`#progress`)는 장식이 아니라 콘텐츠의 5번 항목(수렴 모델 — 극한값에 점근적으로 가까워짐)을 시각적으로 구현한 것. 다른 은유 없이 이걸 유지.
- 번호(01~53, 01~05 등)는 실제로 순서/논리 구조가 있는 콘텐츠라서 붙인 것 (디자인 스킬 원칙상 순서 없는 콘텐츠에 번호 매기기는 지양). 새 섹션 추가 시도 이 기준 유지.

## 콘텐츠 원칙 — 익명화 규칙

**`index.html` (CV)**: 학력·수상·자격증·기타활동은 실명 그대로 (이미 공개된 크레딧이라 문제없음). **경력 섹션의 회사명만 업종/분야로 익명화** (예: "NPL Lounge" → "부동산 경매·NPL 교육 플랫폼"). 새 경력 추가 시 이 규칙 유지.

이메일·전화번호는 의도적으로 넣지 않음 (스팸/사칭 리스크). 연락처 추가하려면 별도 연락용 주소나 폼 링크 권장.

**`values.html`**: 연인·부모·사업파트너·회사명 전부 역할로 표현 (여자친구, 부모님, 공동대표, 사업 파트너 등). 새 항목 추가 시 동일 규칙 유지.

## 배포

정적 파일이라 빌드 스텝 없음. Vercel 기준:

```bash
git init   # 이미 돼 있으면 생략
git add .
git commit -m "update"
git remote add origin <본인 GitHub repo 주소>
git push -u origin main
```

이후 Vercel에서 그 GitHub repo를 import하면 root의 `index.html`을 자동으로 홈으로 잡고, push할 때마다 자동 재배포됨. `vercel.json` 같은 설정 파일 필요 없음 (순수 정적 사이트).

## 다음에 이어서 작업할 때

- 새 페이지를 추가하면 위 컬러/폰트/잉크마크 규칙을 그대로 복사해서 씀 (각 파일이 독립적인 `<style>`을 갖는 구조라 공용 CSS 파일로 뽑고 싶으면 `shared.css`로 분리해도 됨 — 아직 안 해둔 상태).
- `values.html`에 항목을 추가하면 `.item` 마크업 패턴(`num` + `h3` + `desc` + `example`)을 그대로 따르고, 10부 구조 중 어디에 속하는지 먼저 판단해서 해당 `<section class="part">` 안에 넣을 것.
