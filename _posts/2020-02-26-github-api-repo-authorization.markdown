---
layout:     post
title:      "GitHub API Repo Authorization"
date:       2020-02-26 17:53:00 +0000
permalink:  'github-api-repo-authorization'
---

Checking user authorization for repos is important for services that integrate with GitHub. I've used it in [CommitCheck](https://commitcheck.com) and may need to implement it in [PRScheduler](https://prscheduler) too.

I needed authorization for CommitCheck's [status page](https://twitter.com/TomKadwill/status/1218280859465342976/photo/1). When a user visits a status page url, It should check that the user actually has repo access. To do that, I used GitHub's permissions API.

### Implementation

At first I tried getting all the user's data from GitHub and parsing their repos. I thought this was the best way to handle authorization, but the right way is to use the permission API:

```
https://api.github.com/repos/:repo/collaborators/:github_username/permission
```

For example, I can check the permissions of `tomkadwill` for `atom-rails`, with:

```
https://api.github.com/repos/tomkadwill/atom-rails/collaborators/tomkadwill/permission
```

Here's the response object:

```
{
   "permission":"admin",
   "user":{
      "login":"tomkadwill",
      "id":907365,
      "node_id":"MEQ6VaXNldjkwNzM2NS==",
      "avatar_url":"https://avatars0.githubusercontent.com/u/907365?v=4",
      "gravatar_id":"",
      "url":"https://api.github.com/users/tomkadwill",
      "html_url":"https://github.com/tomkadwill",
      "followers_url":"https://api.github.com/users/tomkadwill/followers",
      "following_url":"https://api.github.com/users/tomkadwill/following{/other_user}",
      "gists_url":"https://api.github.com/users/tomkadwill/gists{/gist_id}",
      "starred_url":"https://api.github.com/users/tomkadwill/starred{/owner}{/repo}",
      "subscriptions_url":"https://api.github.com/users/tomkadwill/subscriptions",
      "organizations_url":"https://api.github.com/users/tomkadwill/orgs",
      "repos_url":"https://api.github.com/users/tomkadwill/repos",
      "events_url":"https://api.github.com/users/tomkadwill/events{/privacy}",
      "received_events_url":"https://api.github.com/users/tomkadwill/received_events",
      "type":"User",
      "site_admin":false
   }
}
```

Inside the response body there are two top level keys: `permission` and `user`. If you're trying to check user permissions, all you care about is the `permission` field.

Here's how I implemented the permission API in CommitCheck (using Ruby):

```ruby
REPO_MEMBER_PERMISSIONS = %w[admin write read].freeze

def requested_repo
  "#{org}/#{repo}"
end

def repo_permission_url
  "https://api.github.com/repos/#{requested_repo}/collaborators/#{current_user.github_username}/permission"
end

def status_request_headers
  {
    'Content-Type' => 'application/json',
    'Accept' => 'application/vnd.github.machine-man-preview+json',
    'Authorization' => "Bearer #{current_user.oauth_token}"
  }
end

def check_authorization
  uri = URI(repo_permission_url)
  req = Net::HTTP::Get.new(uri, status_request_headers)
  res = Net::HTTP.start(uri.hostname, uri.port, use_ssl: uri.scheme == 'https') do |http|
    http.request(req)
  end

  REPO_MEMBER_PERMISSIONS.include?(JSON.parse(res.body)['permission'])
end
```

`requested_repo`, `repo_permission_url` and `status_request_headers` build up the request url and headers. `check_authorization` calls the API to check whether the user has `'read'`, `'write'`, `'admin'` or `nil` permissions.

For this particular use case I don't need to differentiate between 'read', 'write' or 'admin'. But if you do then you can change the logic to check for a specific permission type.

