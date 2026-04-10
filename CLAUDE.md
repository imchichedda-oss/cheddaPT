# CLAUDE.md

이 파일은 Claude Code가 이 저장소에서 작업할 때 참고하는 가이드입니다.

## 프로젝트 개요

임채현 포트폴리오 웹사이트 프로젝트입니다. Figma 디자인을 HTML/CSS로 변환한 반응형 웹사이트입니다. 자바스크립트 없이 HTML과 CSS만 사용합니다.

- **Figma 설계 파일**: `https://www.figma.com/design/LzphdfU42jFbW2sH4fl1Ma/...`
- **사용 기술**: HTML5, CSS3 (자바스크립트, 빌드 도구 없음)
- **글꼴**: Noto Sans KR (한국어), Inter (영문), Manrope (제목)
- **반응형 기준**: 1920px (데스크톱), 768px (태블릿), 480px (모바일)

## Figma에서 HTML/CSS로 변환하기

### 디자인 정보 가져오기

사용자가 Figma 노드 ID를 제시할 때 (예: `node-id=5-394`):

1. **Figma에서 메타데이터 추출:**
   ```bash
   curl -s "https://api.figma.com/v1/files/LzphdfU42jFbW2sH4fl1Ma/nodes?ids=5-394&depth=8" \
     -H "X-Figma-Token: YOUR_FIGMA_TOKEN"
   ```

2. **스크린샷 가져오기 (시각적 확인용):**
   ```bash
   curl -s "https://api.figma.com/v1/images/LzphdfU42jFbW2sH4fl1Ma?ids=5-394" \
     -H "X-Figma-Token: YOUR_FIGMA_TOKEN"
   ```

3. **레이아웃 속성을 CSS로 변환:**
   - Figma `layoutMode: VERTICAL` → CSS `flex-direction: column`
   - Figma `layoutMode: HORIZONTAL` → CSS `flex-direction: row`
   - Figma `itemSpacing: 15` → CSS `gap: 15px`
   - Figma `paddingTop/Right/Bottom/Left` → CSS `padding: 위 우 하 좌`

## CSS 구조

### 파일 구성

- **index.html**: 전체 페이지 (의미론적 HTML 요소 사용)
- **style.css**: 모든 스타일 (섹션별 주석으로 구분)
- **img/**: 로컬 썸네일 이미지 (예: `thumb-spotify.jpg`)

### CSS 구성 방식

스타일은 섹션별로 나뉘어 있으며, 각 규칙에는 Figma 설계 참고가 주석으로 남아있습니다:

```css
/* 카드 전체 상자 — 피그마: Large, 550x540, white, r=24, 안쪽 패딩 8px */
.card {
  width: 550px;
  background-color: #ffffff;
  border-radius: 24px;
  padding: 8px;
}
```

### 디자인 값

**색상:**
- 주요 보라색: `#754ea4` (제목), `#805da9` (버튼)
- 배경: `#f2f2f2` (페이지), `#ffffff` (카드)
- 텍스트: `#212121` (진함) ~ `#c5c5c5` (옅음)
- 카드 보조: `#e8e8e8` (태그), `#d4d4d4` (플레이스홀더)

**글자 크기:**
- 큰 제목 (About): `clamp(60px, 8vw, 119px)` (반응형)
- 소개 문구: `4.95vw` (화면 크기에 따라 변함)
- 카드 제목: `25px` / 굵기 700
- 본문: `16px` / 굵기 400

**간격:**
- 섹션 간 큰 간격: `70px`
- 카드 그리드 간격: `30px`
- 카드 내부 간격: `15px` ~ `17px`
- 외부 패딩: `100px`, 내부 패딩: `8px` ~ `14px`

**크기:**
- 카드 너비: `550px` (고정 2열)
- 카드 이미지 높이: `387px`
- 외부 모서리: `24px`, 이미지 모서리: `20px`

## 카드 컴포넌트

Projects와 Others 섹션에서 같은 구조 사용.

**HTML 구조:**
```html
<article class="card">
  <div class="card-img-wrap">
    <img src="img/thumb-name.jpg" alt="설명" class="card-img">
  </div>
  <div class="card-body">
    <div class="card-title-row">
      <h4 class="card-title">제목</h4>
      <div class="card-tools">
        <span class="tool-tag">도구1</span>
        <span class="tool-tag">도구2</span>
      </div>
    </div>
    <div class="card-infobox">
      <p class="card-desc">설명 텍스트</p>
      <span class="card-date">2026.02.03~2026.02.13</span>
    </div>
    <div class="card-btns">
      <button class="storyboard-btn">스토리보드 보기</button>
      <button class="watch-btn">영상 보기</button>
    </div>
  </div>
</article>
```

**CSS:**
- `.card`: 550px 흰색, 8px 패딩, 24px 모서리
- `.card-img-wrap`: 387px 높이, 20px 모서리, 이미지 자동 크기 조정
- `.card-body`: 세로 배치, 17px 간격, 14px 패딩
- `.card-title-row`: 가로 배치, 제목 왼쪽 + 도구 오른쪽 (`margin-left: auto`)
- `.card-infobox`: 세로 배치, 12px 간격
- `.card-btns`: 가로 배치, 가운데 정렬, 15px 간격

**버튼 색상:**
- 스토리보드 (회색): `background: #f4f4f4`, `color: #212121`
- 영상 보기 (보라): `background: #805da9`, `color: #f2f2f2`

## HTML 여러 항목 한꺼번에 수정

카드 7개처럼 비슷한 요소 여러 개를 수정할 때는 Python 사용:

```bash
python3 << 'PYEOF'
with open('index.html', 'r', encoding='utf-8') as f:
    content = f.read()

import re
# 정규식으로 찾기 + 함수로 교체
new_content = re.sub(기존패턴, 교체함수, content, flags=re.DOTALL)

with open('index.html', 'w', encoding='utf-8') as f:
    f.write(new_content)
PYEOF
```

## 반응형 디자인

**태블릿 이하 (768px):**
- 카드 그리드: 2열 → 1열
- 카드 이미지 높이: 387px → 260px
- 섹션 패딩: `70px 100px` → `50px 20px`
- 버튼: 필요시 세로 쌓기

**모바일 이하 (480px):**
- 히어로 높이 더 줄이기
- 버튼 2개 세로 정렬
- 카드 너비 100% (자동 맞춤)

## HTML 규칙

- **의미론적 태그 사용**: `<section>`, `<article>`, `<footer>` (무의미한 `<div>` 남발 금지)
- **클래스 이름**: 소문자 영문 + 하이픈 (예: `card-title-row`)
- **이미지 필수**: 모든 이미지에 `alt` 속성 필수
- **주석**: 섹션 시작/끝에 `<!-- ===================== 섹션명 ===================== -->` 표시

## CSS 클래스 명명 규칙

모두 소문자 영문 + 하이픈 사용. 역할이 명확하도록.

예시:
- `.card-img-wrap` (카드 이미지 감싸는 상자)
- `.card-title-row` (제목 + 도구 행)
- `.card-infobox` (설명 + 날짜 상자)
- `.tool-tag` (도구 태그)
- `.card-btns` (버튼 묶음)

## 이미지 처리

**로컬 이미지 (Figma 플레이스홀더 대체 예정):**
- 경로: `img/` 폴더
- 이름 규칙: `thumb-[프로젝트명].jpg` (소문자, 공백 없음)
- 예: `thumb-spotify.jpg`, `thumb-kuronsan.jpg`
- CSS: `object-fit: cover` + `object-position: center` (일정하게 자르기)

**Figma CDN 이미지 (임시/외부 콘텐츠):**
- Others 섹션 공연, 음원 등
- URL: `https://figma-alpha-api.s3.us-west-2.amazonaws.com/images/[UUID]`
- 다운로드 금지, 직접 링크 사용

## 페이지 섹션

1. **히어로** (`.hero`): 전체 높이 영상 배경
2. **소개** (`.introduce`): 두 줄 글씨, 색깔과 굵기 다름
3. **자기소개** (`.about`): 중앙 제목 + 3단 정보
4. **프로젝트** (`.projects`): 2개 카테고리, 2열 카드 그리드
5. **기타** (`.others`): 2x2 카드 그리드 (공연, 음원)
6. **꼬리말** (`.site-footer`): 단순 텍스트

## 중요 세부사항

- **영상 끝**: 마지막 프레임에서 정지 (반복 금지), `<video autoplay muted playsinline></video>` 사용
- **간격 트릭**: 레이블 너비 맞추기용 `.ls-wide` (11px 간격)
- **줄바꿈 제어**: 소개 문구는 `<span class="introduce-line-kr">` + `display: block` + `white-space: nowrap`
- **글자별 색상**: Figma에서 한글은 검정(`#000000`), 영문은 보라(`#6b3fa0`)
- **자바스크립트 없음**: CSS만 사용, 상호작용은 hover 효과만
- **설계값 정확도**: 모든 색상, 크기, 간격은 Figma와 정확히 맞춤

## 버전 관리

현재 git 저장소가 아님. 파일을 직접 수정하며 진행. 필요시 수정 내용 기록.
