---
layout: default
title: Blog
permalink: /blog/
---

<style>
  .blog-filters {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    margin-bottom: 2rem;
  }

  .blog-filters input,
  .blog-filters select {
    padding: 0.6em 1em;
    font-size: 1rem;
    border: 1px solid #ccc;
    border-radius: 6px;
    background-color: #f9f9f9;
    transition: border 0.2s ease-in-out, background 0.2s;
    flex: 1 1 300px;
  }

  .blog-filters input:focus,
  .blog-filters select:focus {
    border-color: #007acc;
    background-color: #fff;
    outline: none;
  }

  #postsList li {
    margin-bottom: 1rem;
    line-height: 1.4;
  }

  #postsList li a {
    font-weight: 600;
    text-decoration: none;
    color: #333;
  }

  #postsList li small {
    color: #888;
    font-size: 0.9rem;
  }
</style>

<h2>Blog</h2>

<div class="blog-filters">
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
</div>

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