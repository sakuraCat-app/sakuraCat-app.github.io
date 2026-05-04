---
layout: default
title: 文章归档
permalink: /archive/
---

<div class="archive-container">
  <h1>文章归档</h1>
  
  <div class="archive-content">
    {% for post in site.posts %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% capture month %}{{ post.date | date: '%m' }}{% endcapture %}
      
      {% if year != prev_year %}
        {% if prev_year %}
          </ul>
        {% endif %}
        <h2 class="archive-year">{{ year }} 年</h2>
        <ul class="archive-list">
        {% assign prev_year = year %}
      {% endif %}
      
      {% if month != prev_month %}
        <li class="archive-month">{{ month }} 月</li>
        {% assign prev_month = month %}
      {% endif %}
      
      <li class="archive-item">
        <span class="archive-date">{{ post.date | date: '%d' }}</span>
        <a href="{{ post.url | relative_url }}" class="archive-link">{{ post.title }}</a>
        {% if post.category %}
          <span class="archive-category">{{ post.category }}</span>
        {% endif %}
      </li>
    {% endfor %}
    </ul>
  </div>
</div>

<style>
.archive-container {
  max-width: 800px;
  margin: 0 auto;
}

.archive-container h1 {
  font-size: 2.5rem;
  margin-bottom: 2rem;
  border-bottom: 2px solid var(--primary-color);
  padding-bottom: 1rem;
}

.archive-year {
  font-size: 1.8rem;
  margin-top: 2rem;
  margin-bottom: 1rem;
  color: var(--primary-color);
}

.archive-month {
  font-size: 1.2rem;
  margin-top: 1rem;
  margin-bottom: 0.5rem;
  color: var(--secondary-color);
  font-weight: bold;
}

.archive-list {
  list-style: none;
  padding: 0;
}

.archive-item {
  padding: 0.75rem 0;
  border-bottom: 1px solid var(--border-color);
  display: flex;
  align-items: center;
  gap: 1rem;
}

.archive-item:last-child {
  border-bottom: none;
}

.archive-date {
  min-width: 40px;
  color: var(--light-text);
  font-weight: bold;
}

.archive-link {
  flex: 1;
  color: var(--text-color);
  font-weight: 500;
}

.archive-link:hover {
  color: var(--primary-color);
}

.archive-category {
  background-color: var(--accent-color);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 1rem;
  font-size: 0.8rem;
}
</style>
