---
layout: default
title: Blog
permalink: /blog/
---

<h2>Blog</h2>

<!-- Search Box -->
<input type="text" id="searchBox" placeholder="Search posts..." style="margin-bottom: 1em; padding: 0.5em; width: 100%; max-width: 300px;" />

<!-- Category Filter -->
<select id="categoryFilter" style="margin-left: 1em; padding: 0.5em;">
  <option value="all">All Categories</option>
  {% assign categories = site.posts | map: "categories" | join: "," | split: "," | uniq | sort %}
  {% for category in categories %}
    {% unless category == "" %}
      <option value="{{ category | downcase }}">{{ category }}</option>
    {% endunless %}
  {% endfor %}
</select>

<!-- Posts List -->
<ul id="postsList">
  {% for post in site.posts %}
    <li data-title="{{ post.title | downcase }}" data-category="{{ post.categories | join: ' ' | downcase }}">
      <a href="{{ post.url }}">{{ post.title }}</a> – 
      <small>{{ post.date | date_to_string }}</small>
    </li>
  {% endfor %}
</ul>

<script>
  const searchBox = document.getElementById("searchBox");
  const categoryFilter = document.getElementById("categoryFilter");
  const posts = document.querySelectorAll("#postsList li");

  function filterPosts() {
    const search = searchBox.value.toLowerCase();
    const category = categoryFilter.value;

    posts.forEach(post => {
      const matchesSearch = post.dataset.title.includes(search);
      const matchesCategory = category === "all" || post.dataset.category.includes(category);
      post.style.display = matchesSearch && matchesCategory ? "list-item" : "none";
    });
  }

  searchBox.addEventListener("input", filterPosts);
  categoryFilter.addEventListener("change", filterPosts);
</script>