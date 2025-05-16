---
layout: default
title: Blog
permalink: /blog/
---

<h2>Blog</h2>

<div class="blog-layout">
  <aside class="blog-sidebar">
    <input type="text" id="searchBox" placeholder="Search posts..." />
    <select id="categoryFilter">
      <option value="all">All Categories</option>
      {% assign all_categories = "" | split: "" %}
      {% for post in site.posts %}
        {% assign all_categories = all_categories | concat: post.categories %}
      {% endfor %}
      {% assign unique_categories = all_categories | uniq | sort %}
      {% for category in unique_categories %}
        {% unless category == "" %}
          <option value="{{ category | downcase }}">{{ category }}</option>
        {% endunless %}
      {% endfor %}
    </select>
  </aside>

  <section class="blog-posts">
    <ul id="postsList">
      {% for post in site.posts %}
        <li 
          data-title="{{ post.title | downcase | escape }}" 
          data-categories="{{ post.categories | join: ',' | downcase }}">
          
          {% if post.image %}
            <img src="{{ post.image }}" alt="{{ post.title }} image">
          {% endif %}

          <a href="{{ post.url }}">{{ post.title }}</a> – 
          <small>{{ post.date | date_to_string }}</small>

          {% if post.teaser %}
            <p>{{ post.teaser }}</p>
          {% else %}
            <p>{{ post.content | strip_html | truncatewords: 30, "..." }}</p>
          {% endif %}
        </li>
      {% endfor %}
    </ul>
  </section>
</div>

<script>
  const searchBox = document.getElementById("searchBox");
  const categoryFilter = document.getElementById("categoryFilter");
  const posts = document.querySelectorAll("#postsList li");

  function filterPosts() {
    const search = searchBox.value.toLowerCase();
    const selectedCategory = categoryFilter.value;

    posts.forEach(post => {
      const title = post.dataset.title;
      const categories = post.dataset.categories.split(',');

      const matchesSearch = title.includes(search);
      const matchesCategory = selectedCategory === "all" || categories.includes(selectedCategory);

      post.style.display = matchesSearch && matchesCategory ? "list-item" : "none";
    });
  }

  searchBox.addEventListener("input", filterPosts);
  categoryFilter.addEventListener("change", filterPosts);
</script>
