---
layout:     post
title:      "Application Error Inbox Zero"
date:       2020-05-11 07:34:00 +0000
permalink:  'application-error-index-zero'
---

When I shipped the first versions of [CommitCheck](https://commitcheck.com) and [PR Scheduler](https://prscheduler.com) I didn't have error monitoring. This was an acceptable tradeoff given my applications were simple but I should have added error monitoring earlier. [sentry.io](https://sentry.io) and [honeybadger.io](https://honeybadger.io) are both great options. The reason it's important is because I don’t want errors to go unnoticed. If I can track every unexpected error in my application then I can systematically reproduce and fix them.

When I first shipped my apps I found errors by trawling through logs. This quickly become unsustainable. I went too long without having error monitoring because I was spending all my time building new features and fixing obvious bugs. Finally, I bit the bullet and installed Sentry. I chose Sentry because it has a generous free plan but I’m not advocating any particular error monitoring tool.

An error reporting tool should record any exception that's raised by your application. My approach is to aim for 'inbox zero', where I try to maintain zero unresolved errors. I try to fix all Sentry errors before I start building new things. Keeping on top of exceptions is really important, if they build up they can become unmanageable. When my list of unresolved errors gets out of control I start to ignore them altogether. Maintaining Sentry inbox zero may sound like overkill but it's not too difficult for small teams or solo developers.

When it comes to fixing errors, I sometimes find that I don't have enough context to implement a fix. Here is an example from CommitCheck:

![Sentry no method error](/assets/application-error-index-zero/sentry-no-method-error.png)

I can see that something has gone wrong in `commits_json_response_body` but that doesn't help me much because `commits_json_response_body` is a remote API call. The bug is not repeatable so it's not easily reproduce. However, I can add some code to improve the context for next time. Here I'm raising the error manually in the extra params:

```ruby
 def last_commit_url
    @last_commit_url ||= "#{base_url}#{commits_json_response_body.last.fetch('sha')}"
  rescue NoMethodError => e
    Raven.capture_message e.message,
                          logger: 'logger',
                          tags: { environment: 'production' },
                          extra: {
                            commits_json_response_body: commits_json_response_body,
                            commits_url: commits_url
                          }

    raise e
  end
```

With this code merged, Sentry will record the API response body so I can debug it more easily. I’ll mark the exception as resolved and wait for it to occur again.

I find maintaining 'Inbox Zero' in Sentry very satisfying because I'm constantly improving the codebase and staying on top of errors.
