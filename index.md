---

layout: home

---



\## Archives de la Newsletter



<ul class="post-list">

&nbsp; {%- for post in site.posts -%}

&nbsp; <li>

&nbsp;   <h3>

&nbsp;     <a class="post-link" href="{{ post.url | relative\_url }}">

&nbsp;       {{ post.title | escape }}

&nbsp;     </a>

&nbsp;   </h3>

&nbsp;   <span class="post-meta">{{ post.date | date: "%-d %B %Y" }}</span>

&nbsp;   {%- if post.excerpt -%}

&nbsp;     <p>{{ post.excerpt }}</p>

&nbsp;   {%- endif -%}

&nbsp; </li>

&nbsp; {%- endfor -%}

</ul>



