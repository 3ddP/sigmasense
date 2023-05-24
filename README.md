# Usage instructions
---

Here are a few instructions for the usage of this repo. This repo contains the files needed to generate a static blog website with [Jekyll](https://jekyllrb.com/). Most of the stuff in here does not have to be touched. You should only interact with the `./_posts` and `./assets/images` directories. Your blog posts in markdown go in the former, while any images used in your post go in the latter. Feel free to create new folders inside `./assets/images` **for your own blog posts**, but you will be responsible for remembering where you put your stuff. **Don't touch any files you didn't put there yourself unless you're 100% sure you know what you're doing.**

## Making blog posts
---

As mentioned above, we'll be making blog posts using markdown. For clarity, give your markdown files the `.md` extension (although `.markdown` is also valid). In order for your markdown file to be processed by Jekyll as a blog post, it needs to start with a [*YAML Front Matter*](https://jekyllrb.com/docs/front-matter/) containing some special information about the post. In our case, we want the front matter to have the form:
```
---
layout: post
title: "Your blog post's title goes here"
date: 2023-05-11 16:55
categories: blog
---
```

In general, you can just copy-paste this block at the top of your markdown and you're golden. A few details are crucial, though. First, the `layout: post` part **cannot be changed.** Don't touch this. It tells Jekyll how to make HTML out of your markdown, and it needs to be done in a special way if we want to be able to add a comment section. The `title: "..."` part is flexible, just write whatever you want to have as a title. The ```date: YYYY-MM-DD HH:MM``` part is finicky. GitHub will not show blog posts for which the date is in the future. This may give you issues if you use the current time in your time zone and then push changes to the repo. I *think* GitHub's internal time zone is UTC, so keep that in mind if you ever notice you pushed a blog post and it doesn't show up. I suggest just doing `date: YYYY-MM-DD` instead, e.g. `date: 2023-05-24`. Finally, the `categories: blog` chunk. You can use anything instead of `blog` here, but it plays an important role. When Jekyll reads your markdown, it will generate HTML files and place them in a particular folder structure that also affects the URL with which you access the post. It goes something like this:
`{{ site.baseurl }}`