---
title: Hosting a Blog Using Github Pages and Jekyll
header:
    image: /assets/images/callum-shaw-TLxaYmixZ3k-unsplash.jpg
    caption: Photo by [Callum Shaw](https://unsplash.com/@callumshaw?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/coffee-macbook?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText)
date: 2020-12-08T19:00:00-00:00
categories:
    - blog
tags:
    - development
    - github
    - blogging
    - jekyll
toc: true
---

On today's blog post I'll guide you through the steps I took to create this site using [github pages](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/getting-started-with-github-pages), [jekyll](https://jekyllrb.com/) and the [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes) theme. In just a matter of minutes you should be able to launch your site and start blogging.

One of the many advantages of hosting with github pages is that it's *free*! Another amazing benefit is that you get free out of the box `https` support if you decide to use your own domain.

I want to give a shout out to the rock star Android developer [Manuel Vivo](https://twitter.com/manuelvicnt) for inspiring me to launch this site!

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Thanks! Yes, it&#39;s powered by Jekyll using the jasper2 theme deployed to GitHub actions and a custom Google domain :)</p>&mdash; Manuel Vivo (@manuelvicnt) <a href="https://twitter.com/manuelvicnt/status/1334725998605180930?ref_src=twsrc%5Etfw">December 4, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

### Prerequisites

This blog post assumes you are familiar using [git](https://git-scm.com/), [github](https://github.com/) and the command line.

For creating and editing posts I use [Sublime Text](https://www.sublimetext.com/) and the [MarkdownEditing](https://github.com/SublimeText-Markdown/MarkdownEditing) package, which I will cover how to setup in this guide.

If you want to use your own domain like [franciscovelazquez.com](https://franciscovelazquez.com) and do not have one, I highly recommend using [Hover](https://hover.com) for purchasing domains. If by any chance you are using GoDaddy (how dare you) know that hover has a feature to automatically transfer your domain.

### What is jekyll?

Jekyll is a powerful and easy to use static site generator that works with github pages. It lets you write posts in markdown and does the conversion to a static html site for you!

Jekyll offers many themes to make your site feel like your own, but do note that github pages only supports a [limited amount](https://pages.github.com/themes/) of themes.

I decided to go with the Minimal Mistakes theme because of it's simplicity and quick setup.

## First step: copy the template

The minimal mistakes jekyll theme offers a [template](https://github.com/mmistakes/mm-github-pages-starter) that you can use to get started right away.

From the template github repository [here](https://github.com/mmistakes/mm-github-pages-starter) click on "Use this template" button.

{% capture fig_img %}
![Use this template button]({{ '/assets/images/blog_template_start_1.png' | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Click on Use this template button.</figcaption>
</figure>

Next you will create a github repository with the naming convention `<user>.github.io`. Since my github user name is `kikin81` my repository name will then be `kikin81.github.io`. You can choose to keep the repository Private.

{% capture fig_img %}
![Create repository]({{ '/assets/images/blog_new_repo_s1.png' | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>You must use <user>.github.io as the repository name. </figcaption>
</figure>

**Note!** The github pages documentation [here](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/creating-a-github-pages-site) offers a detailed guide on how to do this step from scratch. Feel free to reference their guide if you get stuck at any point.
{: .notice--info}

## Next: Configure repository as github pages

The next step is to navigate to the settings page of our newly created repository and enable github pages functionality.

{% capture fig_img %}
![Navigate to repo settings]({{ '/assets/images/blog_fig_3_settings.png' | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Navigate to the settings page for your repository. </figcaption>
</figure>

{% capture fig_img %}
![Enable github pages]({{ '/assets/images/blog_fig_4_pages.png' | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Enable github pages and select the master branch.</figcaption>
</figure>

After completing these steps you should see a success message `"Your site is ready to be published at https://kikin81.github.io/"`!

## Configure your site

In this step we will clone the repository and configure a few things.

{% capture fig_img %}
![Enable github pages]({{ '/assets/images/blog_fig_5_clone.png' | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Grab the ssh link and clone your repository locally.</figcaption>
</figure>

To clone you need to run the following commands in terminal:

```
$ git clone <ssh-url-to-repo>
$ cd username.github.io
```

{% capture fig_img %}
![Jekyll blog file structure]({{ '/assets/images/blog_fig_6_file_structure.png' | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Take a look at the jekyll site file structure.</figcaption>
</figure>

Next you want to edit the `_config.yml` file with as much detail as possible.
Important things to edit are the site title, description locale and time zone. Personal information that you might want to add is email, twitter user name.

You can also configure google analytics comments and a lot more! For the full list of site configuration settings, please see the documentation [here](https://mmistakes.github.io/minimal-mistakes/docs/configuration/).

You can see that the template came with some blog `Posts` and `About Me` page. I recommend you copy those files out so you can reference them later.

## Create your first blog post

To create a [blog post](https://jekyllrb.com/docs/posts/) you need to create a new file in the `_posts` directory and follow the naming convention:

```
YEAR-MONTH-DAY-title.MARKUP
```

Where `YEAR` is a four digit number, `MONTH` and `DAY` are both two-digit numbers. `MARKUP` is the file extension, I personally use `.md`

Go ahead a create a `hello world post`:

```
2020-12-08-hello-world.md
```

Edit the post to include the following:

```
---
title: "Hello world from Jekyll!"
---

# Welcome!

**Hello world**, this is my first jekyll blog post.
```

Next add all the edited files to version control and push them to master!

```
$ git add .
$ git commit -am "my first blog post!"
$ git push origin master
```

Give github a few seconds and you should be able to see your new post live!

## Optional: Set a custom domain

If you already own a custom domain like `franciscovelazquez.com` then you can easily set it to go to your new blog following these steps.

**Note!** github pages documentation has a detailed guide on configuring custom domain [here](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site). Feel free to refer to their guide if you feel lost.
{: .notice--info}

The first step is to go to your github repository settings, scroll down to the `GitHub Pages` section and set your custom domain there.

{% capture fig_img %}
![Add your domain in settings]({{ '/assets/images/blog_fig_7_domain.png' | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Add your custom domain in the repository settings.</figcaption>
</figure>

For the next part you will need to go to your domain registrar website and add the following ip address as `Type A` (see [doc](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site) for the ip list):

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Finally add a CNAME for your github pages url.

```
CNAME    www    kikin81.github.io
```

Your settings should look like the following:

{% capture fig_img %}
![Domain settings]({{ '/assets/images/blog_fig_8_hover.png' | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Domain dns settings.</figcaption>
</figure>

Once you update your domain DNS settings, give it a few minutes and you should be able to access your github pages from your custom domain!

## Closing thoughts

Github pages and jekyll offer a simple and free way to host your own personal site.

This guide focused on using the Minimal Mistakes Jekyll theme but there are a lot more themes out there so feel free to explore.

If you have any questions feel free to reach me at [@kikin81](https://twitter.com/kikin81) on twitter.

