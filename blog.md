---
layout: default
title: Blog
permalink: /blog/
---

<h2>Blog</h2>

<!-- Search + Filter UI -->
<div style="margin-bottom: 1.5em;">
  <input type="text" id="searchBox" placeholder="Search posts..." style="padding: 0.5em; width: 60%; max-width: 300px;" />

  <select id="categoryFilter" style="padding: 0.5em; margin-left: 1em;">
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
</div>

<!-- Posts List -->
<ul id="postsList">
  {% for post in site.posts %}
    <li 
      data-title="{{ post.title | downcase | escape }}" 
      data-categories="{{ post.categories | join: ',' | downcase }}">
      <a href="{{ post.url }}">{{ post.title }}</a> – 
      <small>{{ post.date | date_to_string }}</small>
    </li>
  {% endfor %}
</ul>

<!-- Filtering Script -->
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