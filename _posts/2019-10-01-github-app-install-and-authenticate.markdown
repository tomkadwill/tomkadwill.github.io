---
layout:     post
title:      "Building GitHub Apps: Authenticate After App Installation"
date:       2019-10-01 07:55:00 +0000
permalink:  'github-app-install-and-authenticate'
last_modified_at:   2019-10-06 12:47:00 +0000
---

Recently I noticed a bug in [PR Scheduler](https://prscheduler.com). Merges weren't triggering GitHub Pages to rebuild. It turns out that rebuilds are not triggered with app tokens, only with personal access tokens. I've fixed the bug now and discovered some new GitHub App features along the way.

GitHub Apps have two different settings obtaining user access tokens: **Request user authorization (OAuth) during installation** & **Setup URL (optional)**.

### Request user authorization (OAuth) during installation

When selected, GitHub will make a callback to your **User authorization callback URL** after every installation. I used this for PR Scheduler because users don't need to visit the website, they can manage everything from the GitHub App page. After configuring it, PR Scheduler was making server callbacks. But there was a problem, OAuth was raising a CSRF failure. The error was due to a missing `state` field. Here's what GitHub say about it:

> Note: If you select Request user authorization (OAuth) during installation when creating or modifying your app, GitHub returns a temporary code that you will need to exchange for an access token. The state parameter is not returned when GitHub initiates the OAuth flow during app installation.

`state` parameters work like this: Your application generates a random string and sends it to the authorization server as part of the auth process. The authorization server sends the state parameter back to the application server. If the `state` parameter received does not match the `state` parameter sent then the request cannot be trusted.

However, In this case, the OAuth request is initiated by GitHub so the application server cannot send a `state` parameter.

If you're getting an error like this, you need to change the configuration of your OAuth library so that it ignores the `state` parameter. I'm using Ruby on Rails and [omniauth-github](https://github.com/omniauth/omniauth-github). To disable the `state` check I used the `provider_ignores_state` setting:

```ruby
config.omniauth :github, 'Iv1.8acded4024678806', '3b4b6c887aba68ec8740136efb6c03902dd37a26', {
  provider_ignores_state: true
}
```

### Setup URL (optional)

This feature works in a similar way to the other. **Request user authorization (OAuth) during installation** performs the OAuth request as part of installation and is handled by GitHub. With **Setup URL (optional)**, GitHub redirects the user to a URL after the installation process finishes, which should initiate the OAuth process.

In this case, you don't have to worry about ignoring `state`. Because the request is coming from your server, your OAuth library will send a `state` parameter.

### So which should you choose?

If your site already has OAuth login then **Setup URL (optional)** will be easier because it plugs straight into your existing OAuth setup. This was the case for [CommitCheck](https://commitcheck.com). However, if you don't have OAuth login on your site (eg: [PR Scheduler](https://prscheduler.com)) you may as well have GitHub handle everything and use **Request user authorization (OAuth) during installation**.