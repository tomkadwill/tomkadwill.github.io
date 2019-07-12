---
layout:     post
title:      "Building an OAuth login using GitHub Apps"
date:       2019-07-12 08:20:00 +0000
permalink:  'oauth-with-github'
---

I recently shipped [CommitCheck](https://www.commitcheck.com/) which allows users to sign in via GitHub. It works via OAuth, which is a standardized way of accessing user account data from external services. There are a few ways to do this in Github: Personal access tokens, OAuth Apps or GitHub Apps. In this blog post I'll explain how to authenticate using a GitHub App. The reason to use an app, as apposed to other methods, is to build a product that others can use, Eg: [Pull Panda](https://github.com/marketplace/pull-panda) or [CommitCheck](https://github.com/marketplace/commitcheck). In this example I'll use Ruby on Rails but the principles can be applied to other languages/frameworks.

Please note, this post does not try to explain OAuth in general. If you don't know how OAuth works go and learn about that before reading this post.

### Creating a GitHub App

Creating an app is simple.
1. Navigate to **Settings**, in GitHub
2. Go to **Developer settings**
3. Go to **GitHub App**
3. Click **New GitHub App**
4. Fill in name, description & homepage url
5. Choose a **User authorization callback URL**. This is the endpoint that GitHub will use to call back to your application with user data
6. You'll also need to specify a Webhook URL. This is the endpoint that GitHub will call when events/hooks (that you've subscribed to) are called. I won't cover webhooks in this post.

GitHub has a [more detailed setup guide](https://developer.github.com/apps/building-github-apps/creating-a-github-app), if you need it.

### Setting up the OAuth flow with Devise

#### 1. Add Devise and Omniauth

Add the [devise](https://github.com/plataformatec/devise) and [omniauth-github](https://github.com/omniauth/omniauth-github) gems to your project and `bundle install`.

#### 2. Configure devise

Devise provides a couple of setup commands. The first for generating configuration files and the second for configuring the devise user:

```
$ rails generate devise:install
$ rails generate devise User
```
You can replace `User` with any model you like.

This will generate a migration, which creates the model with Devise fields.

#### 3. Configure the omniauth gem

Add the following line to `config/initializers/devise.rb`:

```ruby
config.omniauth :github, 'YOUR_CLIENT_ID', 'YOUR_CLIENT_SECRET'
```

To obtain your client_id and client_secret, navigate to the 'general' section of your app.

![github-client-id-and-secret](/assets/github-client-id-and-secret.png)

### 4. Add OAuth logic

Add a new route for authenticating via GitHub:

```ruby
devise_for :users, controllers: { omniauth_callbacks: 'users/omniauth_callbacks' }, skip: [:session]
```

This route will be called by GitHub in order to return the user data. Next add a controller and action for the route:

```ruby
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController
  def github
    if github_user.persisted?
      sign_in_and_redirect github_user, event: :authentication
      # Maybe set a flash message here
    else
      # Do something if user creation fails
    end
  end

  private

  def github_user
    @github_user ||= User.from_omniauth(request.env['omniauth.auth'])
  end
end
```

The controller calls `User.from_omniauth`, you need to define that method on the `User` model.

```ruby
def self.from_omniauth(auth)
  where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
    user.email = auth.info.email
    user.password = Devise.friendly_token[0, 20]
    user.github_username = auth.extra.raw_info.login
  end
end
```

You'll also need to generate a migration that adds the new fields to your User model.

```ruby
class AddOmniauthToUsers < ActiveRecord::Migration[5.2]
  def change
    add_column :users, :provider, :string
    add_column :users, :uid, :string
    add_column :users, :github_username, :string
    change_column :users, :email, :string, null: true
  end
end
```

`provider`, `uid`, `github_username` & `email` are the fields that I think are most important but you can change the migration and `.from_omniauth` method if you want to use different ones. Just check the auth hash returned from GitHub to see what's available.

### 5. Add login button

With everything in place, you can add the login button to your homepage:

```erb
<%= link_to "Sign in with GitHub", user_github_omniauth_authorize_path %>
```

When users click this button they will be re-directed to GitHub's login page. They will then be redirected back to your application. By default, `sign_in_and_redirect` will redirect users to your root route. You can override that by defining `after_sign_in_path_for`.