---
title: Create Pull Requests from the Command Line
header:
    image: /assets/images/yancy-min-842ofHC6MaI-unsplash.jpg
    caption: Photo by [Yancy Min](https://unsplash.com/@yancymin?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText) on [Unsplash](a href="https://unsplash.com/s/photos/github?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText)
date: 2020-12-06T20:00:00-00:00
categories:
    - blog
tags:
    - development
    - github
toc: true
---

Hello there fellow [Github](https://github.com/) user! Do you ever wish you could open pull requests right from the command line? Are you tired of having to fill out common information like "assignee", "reviews" and "tags"? Well, you are in luck because using the [hub](https://hub.github.com/) extension you will be able to power up your github skills.

## what is hub?

[Hub](https://hub.github.com/) is a command-line library that uses the [github rest api](https://docs.github.com/en/free-pro-team@latest/rest) to let you do convenience calls like cloning, forking, viewing active prs and even create pull requests all from terminal.

## installation

If your platform of choice is `macOS` and are already using homebrew you can install it by running the following command

```
$ brew install hub
```

Not using `macOS`? (How dare you!), fret not, the [github page](https://github.com/github/hub#installation) offers installation instructions for a dozen different operating systems.

## authentication

The first time you use the `hub` command it will ask you to authenticate via `username` and `password` and it will then write the oauth token to ` ~/.config/hub`

If you prefer you could also create a custom `oauth` token from the [github oauth site](https://github.com/settings/tokens) under **Developer Settings** and then **Personal tokens**. You need to give it at least the `repo` scope.

After generating the token, edit your `~/.config/hub` file as follows

```
github.com:
- user: YOUR_GITHUB_USER_NAME
  oauth_token: YOUR_OAUTH_TOKEN
  protocol: https
```

## creating a pull request

Finally you are ready to create a pull request with the [pull-request](https://hub.github.com/hub-pull-request.1.html) command!

As an example you can use the following:

```
$ hub pull-request -o -p -a kikin81 -r jbir789 -l bug -M 1
```

Here is what each parameter does:

`-o` This means open the pull request in your default browser.

`-p` Pushes your current branch to `<HEAD>` before creating the pull request.

`-a` Sets the assignee field

`-r` Sets the reviewers (comma separated value)

`l` Sets the labels (comma separated)

`-M` Sets the milestone

**Note!** After entering the `hub pull-request` command you will proceed to your default text editor (i.e vim or emacs) to create the *title* and *message* for the pull request.
The text up to the first blank line is treated as the pull request title, and the rest is used as pull request description in Markdown format.
{: .notice--info}

## github enterprise

Hub also works with github enterprise. You will only need to edit the `~/config/hub` file and add your enterprise domain and oauth token.

Sample configuration:

```
github.my.company.io:
- user: velazquez
  oauth_token: MY_OAUTH_TOKEN
  protocol: https
```

### final thoughts

Hub is a powerful github command-line tool that will speed up your pull request creation.
