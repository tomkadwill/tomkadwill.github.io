---
layout:     post
title:      "Setting Up A Heroku Project With Separate Staging And Master Apps"
date:       2018-01-02 13:12:00 +0000
tags:       books
---

Recently I started working on a new project, which had a separate staging and production Heroku app. The project didn't have much documentation so I needed to figure out how to manage staging and production deploys. Here is what I did:

### Staging setup

1) Add a new remote, named staging, with the staging app url.

```
$ git remote add staging https://git.heroku.com/staging-app.git
```

2) To deploy, push to staging remote. Heroku only deploys code that is pushed to master so you'll need to use the `:` syntax to let Heroku know that you want to push your local `staging` branch to Heroku's `master`.

```
$ git push staging staging:master
```

You can deploy any branch to staging by changing the name of the branch:

```
$ git push staging <other_branch>:master
```

### Master setup

1) Add a new remote, named production, with the production app url.

```
$ git remote add production https://git.heroku.com/production-app.git
```

2) To deploy, push to production remote. I'm assuming that you use master as your 'production' branch.

```
$ git push production master
```

To deploy a different branch to production, you'll need to use the `:` syntax.

```
$ git push production <other_branch>:master
```

### Running Migrations

After a deploy, you may need to run migrations. You don't need to use the git remotes for this; use Heroku's command line tool, specifying the name of the app.

```
$ heroku run rake db:migrate --app staging-app
$ heroku run rake db:migrate --app production-app
```

### Further Reading
* [Heroku docs](https://devcenter.heroku.com/articles/git#deploying-code)
* [mentalized.net](http://mentalized.net/journal/2017/04/22/run-rails-migrations-on-heroku-deploy/)
