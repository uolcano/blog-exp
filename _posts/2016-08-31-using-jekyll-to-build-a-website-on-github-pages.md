---
title: Using jekyll to build a website on GitHub Pages
data: 2016-08-31
category: Site-building
tags:
- jekyll
- GitHub Pages
---

## Installation
1\. ruby

{% highlight bash %}
curl -L https://get.rvm.io | bash -s stable --autolibs=enabled --ruby
{% endhighlight %}

2\. bundler, used to fix the jekyll gems dependencies

{% highlight bash %}
gem install bundler
{% endhighlight %}

3\. jekyll

{% highlight bash %}
gem install jekyll

vim Gemfile # add under two lines to Gemfile
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll-plugins

bundle install
{% endhighlight %}

4\. git

{% highlight bash %}
sudo apt-get install git # on ubuntu
git config --global color.ui true
git config --global user.name 'your name'
git config --global user.email 'your@email.com'
git config -l # list the configuration

ssh-keygen -rsa -C 'your@email.com'
cat ~/.ssh/id_rsa.pub # copy and paste the public key into the GitHub Setting->SSH and GPG keys->SSH keys->new SSH key
ssh -T git@github.com # connect your GitHub account

git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit" # set a pretty git log
{% endhighlight %}

## Site repository
1\. clone a theme

{% highlight bash %}
git clone git@github.com:jekyll/aThemeRepo.git # clone a theme
mkdir blog
cp aThemeRepo/* blog
rm -rf .git
{% endhighlight %}

2\. or build a clean template

{% highlight bash %}
jekyll new blog # quickly create
bundle exec jekyll new blog --force # or stably create
cd blog

vim Gemfile # edit Gemfile
source 'https://ruby.taobao.org/' # perhaps need to set ENV_VAR, the primary source is https:/rubygems.org
'jekyll', '3.2.1' # comment it
gem 'github-pages', group: :jekyll_plugins # uncomment it

bundle install
{% endhighlight %}

3\. change the `_config.yml`

{% highlight yml %}
name: 'blog name'
description: 'your profile'
encoding: 'utf-8'
defaults:
  -
    scope:
      path: ''
      type: 'posts'
    value:
      layout: 'the name of post.html or default.html in ./_layouts path'
      author: 'yourname/nickname'
url: 'http(s)://your.site.com'
baseurl: '/your/project/path or blank'

social:
  github: 'yourname/nickname'
  codepen: 'yourname/nickname'
  rss: feed.xml

disqus: 'yourname/nickname'
comments: true
{% endhighlight %}

4\. change `_includes/social.html` for the social buttons

5\. change `_includes/footer.html` for the footer information

6\. change `_includes/sidebar.html` for the blog sidebar

7\. set theme.scss for the change of `_includes/*`

8\. write your posts in `_posts` directory
name the post files like `2016-08-31-building-a-website.md`

the header of the posts:

{% highlight text %}
---
layout: post
title: your post title
date: year-month-day
---
{% endhighlight %}

9\. push into your site repository

{% highlight bash %}
git init
git branch --orphan gh-pages # currently master branch also can deploy the GitHub Pages

jekyll build # generate the page files from your markdown files
bundle exec jekyll build # or stably build
jekyll serve # preview the site locally
bundle exec jekyll serve # or statby preview

git add --all
git commit -m 'initial commit'
git remote add origin git@github.com:yourname/blog.git
git push origin gh-pages
{% endhighlight %}

Then, all done. You can visit your website by yourname.github.io/blog
Each time you publish your posts, you just need:

{% highlight bash %}
jekyll build # quickly generate page files
git add --all
git commit -m 'commit description'
git push origin gh-pages
{% endhighlight %}

## More
- [Using Jekyll as a static site generator with GitHub Pages](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/)
- [jekyllrb docs](https://jekyllrb.com/docs/home/)
- [jekyll vs bundle exec jekyll](https://github.com/jekyll/jekyll-help/issues/225)