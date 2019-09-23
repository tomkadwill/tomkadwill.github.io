---
layout:     post
title:      "Building GitHub Apps: Authenticate After App Installation"
date:       2019-10-01 07:55:00 +0000
permalink:  'github-app-install-and-authenticate'
---

Recently I noticed a bug in [PR Scheduler](https://prscheduler.com). Merges weren't triggering GitHub Pages to rebuild. It turns out that rebuilds are not triggered with app tokens, only with personal access tokens. I've fixed the bug now and discovered some new GitHub App features along the way.

In order to get a user's access token they must authenticate via OAuth. GitHub Apps have a setting for this: **Request user authorization (OAuth) during installation**. When selected, GitHub will make a callback to your **User authorization callback URL** after every installation.

After I configured it, PR Scheduler was making server callbacks. But there was a problem, OAuth was raising a CSRF failure. The error was due to a missing `state` field. Here's what GitHub say about it:

> Note: If you select Request user authorization (OAuth) during installation when creating or modifying your app, GitHub returns a temporary code that you will need to exchange for an access token. The state parameter is not returned when GitHub initiates the OAuth flow during app installation.

`state` parameters work like this: Your application generates a random string and sends it to the authorization server as part of the auth process. The authorization server sends the state parameter back to the application server. If the `state` parameter received does not match the `state` parameter sent then the request cannot be trusted.

However, In this case, the OAuth request is initiated by GitHub so the application server cannot send a `state` parameter.

If you're getting an error like this, you need to change the configuration of your OAuth library so that it ignores the `state` parameter. I'm using Ruby on Rails and [omniauth-github](https://github.com/omniauth/omniauth-github) for PR Scheduler. omniauth-github will throw an error if `state` is missing. To disable the `state` check I used the `provider_ignores_state` setting:

```ruby
config.omniauth :github, 'Iv1.8acded4024678806', '3b4b6c887aba68ec8740136efb6c03902dd37a26', {
  provider_ignores_state: true
}
```