# birdf00t.github.io

Jekyll로 만든 개인 블로그입니다. GitHub Pages에서 그대로 호스팅됩니다.

## 구조

- `_posts/` — 글 (파일명: `YYYY-MM-DD-제목.md`)
- `_data/profile.yml` — 프로필 정보 (이메일, GitHub 링크 등) → 홈 화면 상단에 표시
- `/` (홈) — 프로필 + 태그 필터 + 글 목록
- `/series/` — 시리즈(카테고리)별 글 목록
- `/write/` — 글쓰기 도구 (제목/시리즈/태그/본문/이미지 입력 → 실시간 미리보기, 파일 다운로드 또는 GitHub에 바로 게시)
- `assets/css/style.css` — 전체 스타일

## 새 글 쓰는 법

`/write/` 페이지에서:

1. 제목, 시리즈(필수), 태그, 본문 작성 (이미지는 붙여넣기/드래그/버튼으로 추가)
2. 오른쪽에서 실시간 미리보기 확인
3. 아래 중 하나로 게시:
   - **GitHub에 바로 게시**: 저장소 쓰기 권한이 있는 내 Personal Access Token으로 터미널 없이 바로 커밋 (다른 사람은 내 토큰이 없으니 게시 불가)
   - **파일 다운로드**: 받은 파일을 `_posts/` 폴더에 넣고 직접 커밋 & 푸시

```bash
git add _posts/YYYY-MM-DD-제목.md
git commit -m "새 글 추가"
git push
```

또는 `_posts/` 안에 직접 마크다운 파일을 만들어도 됩니다. front matter 예시:

```yaml
---
layout: post
title: "글 제목"
series: 카테고리명
tags: [태그1, 태그2]
---
```

## GitHub에 올리기 (최초 1회)

1. GitHub에서 `birdf00t.github.io` 이름으로 새 저장소 생성 (Public)
2. 아래 명령어 실행:

```bash
cd /Users/birdf00t/Downloads/birdf00t.github.io
git init -b main
git add .
git commit -m "Initial commit: Jekyll blog"
git remote add origin https://github.com/birdf00t/birdf00t.github.io.git
git push -u origin main
```

3. 몇 분 후 https://birdf00t.github.io 에서 확인 가능
   (Settings → Pages에서 Build and deployment가 "GitHub Actions" 또는 "Deploy from a branch(main)"으로 되어있는지 확인)

## 로컬 미리보기 (선택)

```bash
bundle install
bundle exec jekyll serve
```

http://localhost:4000 에서 확인.

## 프로필 수정

`_data/profile.yml` 파일에서 이메일, GitHub 링크 등을 수정하세요.

## 글쓰기 도구의 GitHub 연동 보안

`/write/` 페이지는 공개 URL이라 누구나 열어볼 수 있지만, 실제로 글을 게시하려면
저장소 쓰기 권한이 있는 Personal Access Token이 필요합니다. 토큰은 브라우저에만
저장되고 서버로 전송되지 않으며, 게시 전에 토큰 소유자의 GitHub 계정이
저장소 소유자(`birdf00t`)와 일치하는지 확인합니다. 다른 사람은 자신의 토큰으로
이 저장소에 쓸 권한이 없으므로 게시할 수 없습니다.
