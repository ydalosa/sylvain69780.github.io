---
layout: default
---
# About
## About me

The purpose of this blog is essentially for me to learn the Github page Markdown language, and the use of [Jekyll](https://jekyllrb.com/docs/posts/) in order to posts some simple articles.

## Articles about Shadertoy

I know that I'am not a professional artist nor a graphic programmer, but I'am a fan of the demo scene and love to watch and try effects on the **Shadertoy** web site, that's why I felt the need to write something about it.

## Other topics

I also made another [Blog](https://sylvain69780.github.io/digital-culture/) in French to initiate children to graphic programming.

# Posts 

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

