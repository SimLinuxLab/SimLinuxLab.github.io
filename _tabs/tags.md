---
layout: tags
icon: fas fa-tags
order: 2
---

Escolha facilmente um assunto desejado através das tags


{% include lang.html %}

<div id="page-tag">
  <h1 class="ps-lg-2">
    <i class="fa fa-tag fa-fw text-muted"></i>
    {{ page.title }}
    <span class="lead text-muted ps-2">{{ page.posts | size }}</span>
  </h1>
  <ul class="content ps-0">
    {% assign sortPost = page.posts | sort:"title" %}
    {% for post in sortPost %}
      <li class="d-flex justify-content-between px-md-3">
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        <span class="dash flex-grow-1"></span>
        {% include datetime.html date=post.date class='text-muted small text-nowrap' lang=lang %}
      </li>
    {% endfor %}
  </ul>
</div>