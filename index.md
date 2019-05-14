---
profile: true
---

Hello! I'm an iOS developer living in sunny (three weeks out of the year) San Francisco. Around 7 years ago I co-founded a small consulting company. I'm the Chief  Platforms Developer at [Velos Mobile](https://velosmobile.com). 

<ul id="post-list">
    <h3>Latest Post</h3>
    {% for post in site.posts limit:1 %}
        <li>
            <a href="{{ "/" | relative_url  }}{{ post.url | remove_first: '/' }}"><aside class="dates">{{ post.date | date:"%b %d" }}</aside></a>
            <a href="{{ "/" | relative_url  }}{{ post.url | remove_first: '/' }}">{{ post.title }} <h2>{{ post.description }}</h2></a>
        </li>
    {% endfor %}
</ul>

{% include footer.html %}