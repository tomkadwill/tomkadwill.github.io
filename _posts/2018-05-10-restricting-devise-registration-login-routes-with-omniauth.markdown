---
layout:     post
title:      "Restricting devise registration and login routes when using OmniAuth"
date:       2018-05-10 07:00:00 +0000
permalink:  'restricting-devise-registration-login-routes-with-omniauth'
---

My current project only has one way to login, via GitHub Omniauth. To handle that, I’m using Devise along with the [omniauth-github](https://github.com/omniauth/omniauth-github) gem.

By default, Devise provides routes and controller actions for registration and login. However, I only want users to be able to register and login via Github so I disabled the Devise defaults. Here’s how I did it:

### Disabling registration
To disable registration, simply remove the `registerable` module from your Devise model class.
```diff
-  devise :database_authenticatable, :registerable,
-         :recoverable, :rememberable, :trackable, :validatable,
+  devise :database_authenticatable, :recoverable, :rememberable, :trackable, :validatable,
```

### Disabling login
To disable login, remove the default Devise login route. Modify the routes file so that the session routes are skipped. The only route that shouldn’t be skipped is sign_out so add that back in manually.
```diff
-  devise_for :users, controllers: { omniauth_callbacks: 'users/omniauth_callbacks' }
+  devise_for :users, controllers: { omniauth_callbacks: 'users/omniauth_callbacks' }, skip: [:session]
+
+  as :user do
+    delete "/sign_out" => "devise/sessions#destroy", :as => :destroy_user_session
+  end
```

Doing this will remove the routes for both login and registration.
