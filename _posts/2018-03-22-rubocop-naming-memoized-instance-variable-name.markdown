---
layout:     post
title:      "Rubocop Naming/MemoizedInstanceVariableName warning"
date:       2018-03-22 16:58:00 +0000
comments:   true
permalink:  'rubocop-naming-memoized-instance-variable-name'
tags:       Ruby Rails
---

Last week I upgraded [Rubocop](https://github.com/bbatsov/rubocop) to 0.53. I ran the test suite and noticed a new warning called [`Naming/MemoizedInstanceVariableName`](http://www.rubydoc.info/gems/rubocop/RuboCop/Cop/Naming/MemoizedInstanceVariableName), which targets methods that use an instance variable for memoization. Here is an example that would trigger the warning:

```ruby
def greeting
  @hello ||= user_greeting
end
```

Rubocop raises an error because the method name is different from the name of the instance variable, used for memoization. This seems sensible so I started working through the failing tests and fixing them. As I fixed tests I began to find bugs. I realised that the `Naming/MemoizedInstanceVariableName` warning is super useful. Here's an example of a bug that I uncovered:

```ruby
def user
  @user ||= User.create!
end

def admin
  @user ||= Admin.create!
end
```

This bug occurred in my test code, probably due to careless copy/pasting. It could occur in application code just as easily. The bug is that calling the `user` method will create a `User` and memoize it. When `admin` is called, instead of creating an `Admin`, the `User` object is returned.

Upgrading ensures that this type of bug will never occur again. Thanks Rubocop!
