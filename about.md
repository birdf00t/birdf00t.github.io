---
layout: default
title: 소개
permalink: /about/
---

<div class="profile">
  <h1>{{ site.data.profile.name }}</h1>
  <p class="profile-bio">{{ site.data.profile.bio }}</p>

  <ul class="profile-links">
    <li>✉️ <a href="mailto:{{ site.data.profile.email }}">{{ site.data.profile.email }}</a></li>
    {% for link in site.data.profile.links %}
    <li>🔗 <a href="{{ link.url }}" target="_blank" rel="noopener">{{ link.label }}</a></li>
    {% endfor %}
  </ul>
</div>

---

이 섹션 위쪽 프로필은 `_data/profile.yml` 파일을 수정하면 자동으로 바뀝니다.
아래 내용은 자유롭게 편집해서 자기소개를 채워보세요.
