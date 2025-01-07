---
layout:     post
title:      "Adding last modified date to Jekyll"
date:       2019-03-28 06:55:00 +0000
permalink:  'adding-last-modified-date-to-jekyll'
---

I decided to add a ‘last modified’ date to my blog posts. The idea is to let readers know when a post was last revised as well as the date it was originally published.

I found a plugin called [jekyll-last-modified-at](https://github.com/gjtorikian/jekyll-last-modified-at) that automatically calculates the date a post last changed. To make it work you just need to add a `{{ "{% last_modified_at " }}%}` tag to your project’s post template.

However, the build failed when I deployed to GitHub pages:

```
The tag `last_modified_at` on line 7 in `/_layouts/post.html` is not a recognized Liquid tag
```

GitHub pages does not support `jekyll-last-modified-at`, although it looks like [they are working on it]( https://github.com/github/pages-gem/pull/119). You can get around this by building locally and pushing to GitHub but I didn’t want to change my deployment process just for one gem.

There is another downside to `jekyll-last-modified-at`. It uses Git history to determine when the file was last modified. This can be problematic when making small changed to multiple posts. For example, you may want to optimise images across all posts. In this case, the content hasn't really been updated but ‘last modified’ will change. I want something that provides more control, even if I have to set the date manually.

The solution I found is to hardcode `last_modified_at` on each post, just like I do for `date`:

```
---
layout:             post
title:              "An awesome blog post"
date:               2017-12-01 23:22:00 +0000
last_modified_at:   2019-03-25 8:30:00 +0000
---
```

I only want to specify `last_modified_at` if the post is being updated. When I write a new post, I'll leave this field blank. In order to make this work I added some conditional logic to the post template.

I had not customised posts before so I copied `_layouts/post.html` from my [blog’s theme](https://github.com/jekyll/minima/) and pasted it to `_layouts/post.html` in my project. With this layout in place I added the conditional logic:

```
{{ "{%- assign date_format = site.minima.date_format | default: '%b %-d, %Y' " }}-%}

{{ "{%- if page.last_modified_at " }}-%}
    Last updated: {{ "{%- page.last_modified_at | date: date_format " }}-%}
{{ "{%- else " }}-%}
    Last updated: {{ "{%- page.date | date: date_format " }}-%}
{{ "{%- endif " }}-%}
```

If the post has a `last_modified_at` date then it will be displayed, otherwise it will display the original published date.

In my case, I want to display the `last_modified_at` date at the top of the post and also have the original published date at the bottom. That way readers know whether the post has been updated recently and also the date it was originally published. My post layout file looks like this:

```
---
layout: default
---
<article class="post h-entry" itemscope itemtype="http://schema.org/BlogPosting">

  <header class="post-header">
    <h1 class="post-title p-name" itemprop="name headline">{{ "{{ page.title | escape " }}}}</h1>
    <p class="post-meta">
      <time class="dt-published" datetime="{{ "{{ page.date | date_to_xmlschema " }}}}" itemprop="datePublished">
        {{ "{%- assign date_format = site.minima.date_format | default: '%b %-d, %Y' " }}-%}

        {{ "{%- if page.last_modified_at " }}-%}
            Last updated: {{ "{{ page.last_modified_at | date: date_format " }}}}
        {{ "{%- else " }}-%}
            Last updated: {{ "{{ page.date | date: date_format " }}}}
        {{ "{%- endif " }}-%}
      </time>
      {{ "{%- if page.author " }}-%}
        • <span itemprop="author" itemscope itemtype="http://schema.org/Person"><span class="p-author h-card" itemprop="name">{{ "{{ page.author | escape " }}}}</span></span>
      {{ "{%- endif " }}-%}
  </header>

  <div class="post-content e-content" itemprop="articleBody">
    {{ "{{ content " }}}}
  </div>

  <p class="post-meta">
    <time class="dt-published" datetime="{{ "{{ page.date | date_to_xmlschema " }}}}" itemprop="datePublished">
        Originally published on: {{ "{{ page.date | date: date_format " }}}}
      </time>
  </p>

  {{ "{%- if site.disqus.shortname " }}-%}
    {{ "{%- include disqus_comments.html " }}-%}
  {{ "{%- endif " }}-%}

  <a class="u-url" href="{{ "{{ page.url | relative_url " }}}}" hidden></a>
</article>
```

### References and links
1. [The PR that will hopefully allow GitHub Pages to build jekyll-last-modified-at](https://github.com/github/pages-gem/pull/119)
2. [add-an-updated-field-to-your-jekyll-site](https://zzz.buzz/2016/02/13/add-an-updated-field-to-your-jekyll-site/)

