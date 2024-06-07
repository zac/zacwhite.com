---
profile: true
---

Hello! 👋 I'm a mobile developer living in the beautiful Bay Area 🌉. Around 13 years ago I co-founded a consulting company, [Velos Mobile](https://velosmobile.com) 💼. I'm their Chief  Platforms Developer. 🏗️📱⌚💻📺🥽

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
