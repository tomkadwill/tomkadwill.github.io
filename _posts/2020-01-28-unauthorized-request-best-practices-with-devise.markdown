---
layout:     post
title:      "Unauthorized request best practices with Devise"
date:       2020-01-28 08:04:00 +0000
permalink:  'unauthorized-request-best-practices-with-devise'
---

I added a new feature to [commitcheck.com](https://commitcheck.com) that allows users to check the [status of their PR](https://twitter.com/TomKadwill/status/1218280859465342976?s=20). For security reasons, users should only have access to status pages that they are authorized to see. I handle authorization using GitHub's API, but I wont cover that now. The thing I want to talk about is the UX for unauthorized requests.

### Good UX

There are two reasons why someone would hit the unauthorized page: they don't have access or they are not signed in.

If the user doesn't have access then the user journey is pretty simple. The user requests the status page and get redirected to an error page, either 404 or 401.

![devise unauthorized flow](/assets/unauthorized-request-best-practices-with-devise/devise-unauthorized.png){:.image-center}

The journey for a logged out users is a little more complicated. The user requests the status page and gets redirected to an error page (ideally the error page should advise them to login). When the user logs in they should be redirected to the page they originally requested.

![devise unauthorized flow login](/assets/unauthorized-request-best-practices-with-devise/devise-unauthorized-login.png){:.image-center}

For example, the user requests `commitcheck.com/status/rails/rails/123` and gets redirected to an error page. When the user logs in they should be redirected back to `commitcheck.com/status/rails/rails/123`.

If the user is trying to request a page they don't have access to, they will continually be redirected to the error page.

### Redirecting to unauthorized page with Devise

I'm using Devise to handle authentication in CommitCheck. Devise doesn't provide this UX out of the box but it's simple to implement.

The first thing I did was ensure that unauthenticated requests hit the error page. To do that I created a custom failure app in `lib/custom_failure.rb`:

```ruby
class CustomFailure < Devise::FailureApp
  def redirect_url
    page_unauthorized_path
  end

  def respond
    if http_auth?
      http_auth
    else
      redirect
    end
  end
end
```

And added it to `config/initializers/devise.rb`:

```ruby
  config.warden do |manager|
    manager.failure_app = CustomFailure
  end
```

I also added lib to my Rails autoload path, in `config/application.rb`:

```ruby
  config.autoload_paths << Rails.root.join('lib')
```

Next I defined the `page_unauthorized` route that I'm calling in `CustomFailure`:

```ruby
get '/401', to: 'home#page_unauthorized', as: :page_unauthorized
```

Finally, I wrote the new controller action in `HomeController` that serves the 401 page:

```ruby
  def page_unauthorized
    render file: 'public/401.html', status: :unauthorized
  end
```

In my case the 401 page has the usual CommitCheck styling and displays an error message, which prompts the user to login:

![devise unauthorized flow page](/assets/unauthorized-request-best-practices-with-devise/devise-unauthorized-page.png)

### Redirecting to previous resource with Devise

In this section I'll explain how I achieved the second user flow with Devise. The aim is to redirect the user back to the path they originally requested, after authenticating.

Devise has a method called `stored_location_for(resource)` which redirects the user back to a stored location. Devise stores a location automatically when a request is marked as unauthenticated. To implement, I updated `HomeController#page_unauthorized` to redirect to the stored location:

```ruby
  def page_unauthorized
    if user_signed_in?
      redirect_to login_redirect_path
    else
      render file: 'public/401.html', status: :unauthorized
    end
  end

  private

  def login_redirect_path
    stored_location_for(:user) || admin_path
  end
```

That's it. Devise makes it simple to build a nice "unauthorized -> login -> redirect" user journey.