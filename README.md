# Usage instructions

Here are a few instructions for the usage of this repo. This repo contains the files needed to generate a static blog website with [Jekyll](https://jekyllrb.com/). Most of the stuff in here does not have to be touched. You should only interact with the `./_posts` and `./assets/images` directories. Your blog posts in markdown go in the former, while any images used in your post go in the latter. Feel free to create new folders inside `./assets/images` **for your own blog posts**, but you will be responsible for remembering where you put your stuff. **Don't touch any files you didn't put there yourself unless you're 100% sure you know what you're doing.**

* **NOTE:** it takes a while for changes to show up on the website. Wait a few minutes after pushing changes to see if they show up correctly. If more than 5~10 minutes go by, an error probably occured. You can check the logs on GitHub by going to the repository, then `Settings > Pages > pages and build deployment > build > Build with Jekyll`. You can also test locally before pushing; more on this later.

---

## Making blog posts
### Front Matter
As mentioned above, we'll be making blog posts using markdown. For clarity, give your markdown files the `.md` extension (although `.markdown` is also valid). In order for your markdown file to be processed by Jekyll as a blog post, it needs to be named `YYYY-MM-DD-post-name.md`, where `post-name` can be anything you want. It also needs to start with a [*YAML Front Matter*](https://jekyllrb.com/docs/front-matter/) containing some special information about the post. In our case, we want the front matter to have the form:
```
---
layout: post
title: "Your blog post's title goes here"
date: 2023-05-11 16:55
categories: blog
---
```

In general, you can just copy-paste this block at the top of your markdown and you're golden. A few details are crucial, though. First, the `layout: post` part **cannot be changed.** Don't touch this. It tells Jekyll how to make HTML out of your markdown, and it needs to be done in a special way if we want to be able to add a comment section. The `title: "..."` part is flexible, just write whatever you want to have as a title. The `date: YYYY-MM-DD HH:MM` part is finicky. GitHub will not show blog posts for which the date is in the future. This may give you issues if you use the current time in your time zone and then push changes to the repo. I *think* GitHub's internal time zone is UTC, so keep that in mind if you ever notice you pushed a blog post and it doesn't show up. I suggest just doing `date: YYYY-MM-DD` instead, e.g. `date: 2023-05-11`. Finally, the `categories: blog` chunk. You can use anything instead of `blog` here, but it plays an important role in how Jekyll builds URLs and how we create links to blog posts.

### How URLs are generated
 When Jekyll reads your markdown, it will generate HTML files and place them in a particular folder structure that also affects the URL with which you access the post. It goes something like this:
```
{{ site.baseurl }}/category/YYYY/MM/DD/post-name.html
```
Here, the `{{ site.baseurl }}` should be left as-is, since it will be interpreted automatically based on the configuration in `_config.yml`; you don't need to worry about it. The `category/YYYY/MM/DD` in the URL comes from the front matter, and the `post-name` comes from the filename (recall the `YYYY-MM-DD-post-name.md` from before). That means that if we create a post with the front matter from the example above, we can create a hyperlink to it as ```[custom hyperlink text]({{ site.baseurl }}/blog/2023/05/11/post-name.html)```.

### Adding images
We can add images to markdown files with a snippet of the form `![some text](path/to/your/image.png)`. Other extensions are also supported. As mentioned earlier, we'll be putting the images in `./assets/images` for the sake of organization. Now, the interesting part is that we have to provide the path based on how the website URLs are constructed, so we have to make a small change. We'll have to use
```
![some text]({{ site.baseurl }}/assets/images/your-image.png)
```
so that the generated HTML can fish out the images correctly.

---
## Adding a comments section
This part is pretty cool. By default, the blog posts have no comments section since Jekyll is static. As explained [here](https://dc25.github.io/myBlog/2017/06/24/using-github-comments-in-a-jekyll-blog.html) and [here](https://github.com/dc25/minimaWithGithubComments), what we're using is the default Jekyll template called `minima` with some modifications that allow us to use GitHub issues to create a comments section. A GitHub repository has to be specified in the `_config.yml` as `github_comments_repository:  your-github-user/your-issues-repo`. This repository can have anything inside. The idea is that we link an issue to a blog post, and then GitHub's issue system is treated as a comments section. To do this, we have to specify an issue in the blog post's front matter. The front matter of a post with a comments section will look like
```
---
layout: post
title: "Your blog post's title goes here"
date: 2023-05-11 16:55
categories: blog
github_comments_issueid: "1"
---
```
where the field `github_comments_issueid: "1"` has been added. The number `"1"` should be the number of the issue in the issues repository. 

For simplicity, I've already created a repository [here](https://github.com/3ddP/sigmasense-comments) and added it to the `_config.yml`. You can use this repo to create issues for your blogs' comments sections. If I don't recognize the user that created the issue, I'll delete it, so beware :P

---
## Using Jupyter notebooks
Many of you like using Jupyter notebooks. The good news is that we can turn notebooks into posts for Jekyll fairly easily! After making your Jupyter notebook as usual, go to `File > Download as > Markdown (.md)` to download a compressed file that contains your notebook in markdown format along with all of the images used in the notebook. After doing this, we need to do a couple of extra steps. First, put the markdown file in the `./_posts` directory and the images in `./assets/images` (you can make a folder one level deeper, name the images whatever you want, etc.). Then, open the markdown file. You'll have to add a front matter like the ones above (in fact, you can copy-paste them from this README and make whichever modifications are needed). Next, look for the lines in the markdown file where the images are imported. Wherever there is a line of the form `![image](image-name.png)`, modify the path into `![image]({{ site.baseurl }}/assets/images/image-name.png)` (or whichever sub-path of this you chose). Done! If you're very gung-ho about this, you could easily set up a script that does this for you automatically... 

---
## Testing locally
You can test the website locally before pushing. This has the advantage of reflecting changes immediately, but setup is required and not everything works exactly as it does on GitHub. You'll first need to clone the repo, but you've probably done this already. You'll also have to install Jekyll. You can find installation instructions [here](https://jekyllrb.com/docs/installation/). I suggest using WSL if you're on Windows. After installing Jekyll (which involves installing Ruby, and then Jekyll and Bundler), go to the root directory of the repository and use the command
```
bundle exec jekyll serve
```
after which Jekyll will tell you how to access the website. More info can be found [here](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/). If you get errors regarding missing `Gems`, it may help to delete your local `Gemfile.lock` and then use the command `bundle install`.