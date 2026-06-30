---
layout: default
title: Posts and Articles
permalink: /posts/
---

<div class="posts-showroom">
  <h1>:Buffer all_logs</h1>

  <div class="search-container" style="margin: 30px 0 40px 0;">
    <input type="text" id="search-input" placeholder="Search logs and articles by title..."
           style="width: 100%; padding: 12px 20px; background-color: #121110; border: 1px solid rgba(197, 176, 136, 0.3); color: #C5B088; font-family: 'Fira Code', monospace; border-radius: 6px; font-size: 1rem;">
  </div>

  {% assign all_entries = site.posts | concat: site.articles | sort: "date" | reverse %}

  <ul class="simple-article-list" id="posts-list">
    {% for entry in all_entries %}
      <li class="article-list-item post-item">
        <span class="article-item-date">{{ entry.date | date: "%b %d, %Y" }}</span>
        <a href="{{ entry.url | relative_url }}" class="article-item-link post-title-link">{{ entry.title }}</a>
      </li>
    {% endfor %}
  </ul>
</div>

<script>
  document.getElementById('search-input').addEventListener('input', function(e) {
    const searchTerm = e.target.value.toLowerCase();
    const posts = document.querySelectorAll('.post-item');

    posts.forEach(post => {
      const title = post.querySelector('.post-title-link').textContent.toLowerCase();
      if (title.includes(searchTerm)) {
        post.style.display = 'flex';
      } else {
        post.style.display = 'none';
      }
    });
  });
</script>
