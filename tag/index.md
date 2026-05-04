---
layout: default
title: 标签浏览
permalink: /tag/
---

<div class="tags-overview">
  <h1>标签浏览</h1>
  
  <div class="tags-cloud-page">
    {% assign all_tags = site.posts | map: 'tags' | join: ',' | split: ',' | uniq | sort %}
    {% for tag in all_tags %}
      {% if tag %}
        {% assign posts_count = site.posts | where_exp: "post", "post.tags contains tag" | size %}
        <a href="/tag/{{ tag }}/" 
           class="tag-item"
           style="font-size: {% if posts_count > 8 %}1.8{% elsif posts_count > 5 %}1.5{% elsif posts_count > 3 %}1.3{% else %}1{% endif %}em;">
          {{ tag }}
          <span class="tag-count">({{ posts_count }})</span>
        </a>
      {% endif %}
    {% endfor %}
  </div>

  <div class="tags-list">
    <h2>按字母排序</h2>
    {% assign sorted_tags = all_tags | sort %}
    {% for tag in sorted_tags %}
      {% if tag %}
        {% assign posts_count = site.posts | where_exp: "post", "post.tags contains tag" | size %}
        <div class="tag-list-item">
          <a href="/tag/{{ tag }}/" class="tag-name">{{ tag }}</a>
          <span class="tag-posts-count">{{ posts_count }} 篇</span>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>

<style>
.tags-overview {
  max-width: 1000px;
  margin: 0 auto;
}

.tags-overview h1 {
  font-size: 2.5rem;
  margin-bottom: 2rem;
  border-bottom: 2px solid var(--primary-color);
  padding-bottom: 1rem;
}

.tags-cloud-page {
  background: var(--bg-color);
  padding: 2rem;
  border-radius: 0.75rem;
  margin-bottom: 3rem;
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  align-items: center;
  justify-content: center;
}

.tag-item {
  background: white;
  border: 2px solid var(--primary-color);
  color: var(--primary-color);
  padding: 0.5rem 1rem;
  border-radius: 1rem;
  transition: all 0.3s ease;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
}

.tag-item:hover {
  background: var(--primary-color);
  color: white;
  transform: scale(1.1);
}

.tag-count {
  font-size: 0.7em;
  opacity: 0.8;
}

.tags-list {
  margin-top: 2rem;
}

.tags-list h2 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  border-bottom: 2px solid var(--primary-color);
  padding-bottom: 0.5rem;
}

.tag-list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem 1rem;
  border-bottom: 1px solid var(--border-color);
  transition: all 0.3s ease;
}

.tag-list-item:hover {
  background: var(--bg-color);
  border-radius: 0.5rem;
}

.tag-name {
  color: var(--primary-color);
  font-weight: 600;
}

.tag-name:hover {
  color: var(--secondary-color);
}

.tag-posts-count {
  color: var(--light-text);
  font-size: 0.9rem;
}

@media (max-width: 768px) {
  .tags-cloud-page {
    justify-content: flex-start;
  }

  .tag-item {
    font-size: 0.9rem;
  }
}
</style>
