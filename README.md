# birdf00t.github.io

Jekyll로 만든 개인 블로그입니다. GitHub Pages에서 그대로 호스팅됩니다.

## 구조

- `_posts/` — 글 (파일명: `YYYY-MM-DD-제목.md`)
- `_data/profile.yml` — 프로필 정보 (이메일, GitHub 링크 등) → `/about/` 페이지에 표시
- `/write/` — 새 글 작성 도우미 (제목/시리즈/태그/본문 입력 → 올바른 형식의 `.md` 파일 다운로드)
- `/series/` — 시리즈(카테고리)별 글 목록
- `/tags/` — 태그별 글 목록
- `assets/css/style.css` — 전체 스타일

## 새 글 쓰는 법

1. 사이트 상단 **글쓰기** 메뉴(`/write/`)로 이동
2. 제목, 시리즈(필수), 태그, 본문 입력 후 **파일 다운로드**
3. 받은 파일을 `_posts/` 폴더에 넣기
4. 커밋 & 푸시

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
