---
layout: default
---
# About
## About me

I'am not a professional artist nor graphic programmer, but I'am a fan of the demo scene and love to watch effects on the **Shadertoy** web site.

[Here](https://www.shadertoy.com/user/sylvain69780) a link to Shadertoy showing my published shaders. There is no comparison with what a professional is able to produce, but I was surprised of the kind comments I may had from the Shadertoy's community.

You may find in [this post]({% post_url 2020-01-01-my-published-shaders %}) a commented list of it.

In [this post]({% post_url 2021-04-02-notes-about-some-shaders %}) a database of of the shaders I noted. With the quatity of awesome publications there is now on this website, it becomes quite impossible to maintain it. 

I also have another [Blog](https://sylvain69780.github.io/digital-culture/) to initiate child and olders to graphic programming.

# Posts 

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

