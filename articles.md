---
layout: default
title: :Buffer Articles
permalink: /articles/
---

<div class="articles-showroom">
  <h1>:Buffer articles</h1>
  <p class="showroom-description">Leituras aprofundadas, análises de arquitetura de software e investigações de segurança.</p>

  <ul class="simple-article-list">
    {% assign sorted_articles = site.articles | sort: 'date' | reverse %}
    {% for article in sorted_articles %}
      <li class="article-list-item">
        <span class="article-item-date">{{ article.date | date: "%b %d, %Y" }}</span>
        <a href="{{ article.url | relative_url }}" class="article-item-link">{{ article.title }}</a>
      </li>
    {% endfor %}
  </ul>
</div>
