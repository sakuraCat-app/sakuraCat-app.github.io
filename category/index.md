---
layout: default
title: 分类浏览
permalink: /category/
---

<div class="categories-overview">
  <h1>分类浏览</h1>
  
  <div class="categories-grid">
    {% assign categories = site.posts | map: 'category' | uniq | sort %}
    {% for category in categories %}
      {% if category %}
        {% assign posts_count = site.posts | where_exp: "post", "post.category == category" | size %}
        <div class="category-card">
          <h3><a href="/category/{{ category }}/">{{ category }}</a></h3>
          <p class="category-count">{{ posts_count }} 篇文章</p>
          <ul class="category-preview">
            {% for post in site.posts %}
              {% if post.category == category %}
                {% if forloop.index <= 3 %}
                  <li><a href="{{ post.url | relative_url }}">{{ post.title }}</a></li>
                {% endif %}
              {% endif %}
            {% endfor %}
          </ul>
          <a href="/category/{{ category }}/" class="category-view-all">查看全部 →</a>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>

<style>
.categories-overview {
  max-width: 1000px;
  margin: 0 auto;
}

.categories-overview h1 {
  font-size: 2.5rem;
  margin-bottom: 2rem;
  border-bottom: 2px solid var(--primary-color);
  padding-bottom: 1rem;
}

.categories-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2rem;
}

.category-card {
  background: white;
  border: 2px solid var(--primary-color);
  padding: 1.5rem;
  border-radius: 0.75rem;
  transition: all 0.3s ease;
}

.category-card:hover {
  box-shadow: var(--shadow-lg);
  transform: translateY(-5px);
}

.category-card h3 {
  font-size: 1.3rem;
  margin-bottom: 0.5rem;
}

.category-card h3 a {
  color: var(--primary-color);
}

.category-card h3 a:hover {
  color: var(--secondary-color);
}

.category-count {
  color: var(--light-text);
  font-size: 0.9rem;
  margin-bottom: 1rem;
}

.category-preview {
  list-style: none;
  margin-bottom: 1rem;
}

.category-preview li {
  padding: 0.5rem 0;
  border-bottom: 1px solid var(--border-color);
}

.category-preview li:last-child {
  border-bottom: none;
}

.category-preview a {
  color: var(--text-color);
  font-size: 0.9rem;
}

.category-preview a:hover {
  color: var(--primary-color);
}

.category-view-all {
  color: var(--primary-color);
  font-weight: 600;
  font-size: 0.9rem;
}

.category-view-all:hover {
  color: var(--secondary-color);
}

@media (max-width: 768px) {
  .categories-grid {
    grid-template-columns: 1fr;
  }
}
</style>
