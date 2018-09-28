---
layout:     post
title:      "When to use git fetch, git pull and git reset —hard"
date:       2018-05-24 07:00:00 +0000
permalink:  'git-reset-or-git-pull'
---

When starting a new feature, I normally checkout a branch from `master`. Before I do this, I always run:

```
$ git fetch
$ git reset —hard origin/master
```

It’s a habit and I realised that I’m not sure why I do it. Is `git fetch` required? How does this differ from `git pull`? What's the difference between `git pull` and `git reset --hard origin/master`? I decided to find out, in this blog post.

### tl;dr
The answer is summarised in this [StackOverflow answer](https://stackoverflow.com/a/43037318/847857). Read on for a more detailed explanation.

### git fetch
`git fetch` downloads commits, files and branches from the git remote. You’ll need to use this command to get the latest changes that others have made. You’ll also need to use it to checkout a new branch that someone else has pushed.

### git pull
`git pull` does two things: `git fetch` and then `git merge origin/<branch>`. This is useful if someone else has made new commits, on your branch, and you want to merge them into your branch. Git will attempt to auto-merge any local changes. If they cannot be resolves, it will result in merge conflicts.

### git reset
Sometimes a branch has diverged from origin so much, that it doesn’t make sense to try to resolve all the conflicts. In this case, it’s better to just reset your local branch to whatever is on origin. To do this, you need to fetch first and then run `git reset —hard origin/<branch>`.

### Conclusion
I can quit my annoying habit of running `git fetch` and `git reset —hard origin/master` every time I checkout a new branch. As long as `master` is clean, it’s fine to run `git pull`.
