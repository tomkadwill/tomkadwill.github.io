---
layout: post
title:  "Tools For Rails CI Build Process"
date:   2017-12-09 14:00:20 +0000
---

A good Rails CI build process should check that code passes all unit tests, before progressing to the next stage of the pipeline. However, there are lots of tools that can give you a greater degree of confidence in your code:

## RuboCop
[Rubocop](https://github.com/bbatsov/ruby-style-guide) is a static code analyzer for Ruby which enforces code style guidelines. For example, two spaces vs tabs. Rubocop comes with a community defined style guide but you can optionally override rules to develop your own custom set of guidelines.

Rubocop is one of my favorite gems. For teams, it allows developers to all write in the same style. This makes code much easier to work with and help foster the feeling that code was written by a single team rather than many different individuals. For solo developers, Rubocop helps you write better code. For example, on error that Rubcop will alert you about: `Use Integer check type of an integer number. Since Fixnum is platform-dependent, checking against it will return different results on 32-bit and 64-bit machines.`

### TODO: enable for CI
### TODO: more info
### TODO: re-read

## Brakeman
[Brakeman](https://github.com/presidentbeef/brakeman) is a static analysis tool which checks Ruby on Rails applications for security vulnerabilities. Here is some example output from Brakeman:

```
Confidence: High
Category: Cross-Site Scripting
Check: CrossSiteScripting
Message: Unescaped model attribute
Code: Post.friendly.find(params[:id]).body
File: app/views/posts/show.html.erb
Line: 3
```

Brakeman is a really important tool for keeping your code secure. It won't find all security problems a significant number of security holes can be fixed with this gem.

### TODO: enable for CI
### TODO: more info
### TODO: re-read

## Bullet

blah

### TODO: enable for CI
### TODO: more info
### TODO: re-read

## rails_best_practices
[rails_best_practices](https://github.com/flyerhzm/rails_best_practices) is used to check code quality in a Rails app. The gem will check for things like: breaking the law or demeter or using .blank? instead of using a query attribute.

### TODO: enable for CI
### TODO: more info
### TODO: re-read


## traceroute

# Figure out why this doesn't work. If it doesn't then report on it. (perhaps outline all the ones that didn't work at the end)

## rack-mini-profiler

### TODO: enable for CI
### TODO: more info
### TODO: re-read
* deadweight (not actively worked on)
* rubycritic (reek, flay, flog) -> I prefer installing them separately
* fasterer
* bundler-audit
* fukuzatsu
* simplecov
* excellent (https://github.com/simplabs/excellent)
* i18n-tasks

refs
* https://infinum.co/the-capsized-eight/top-8-tools-for-ruby-on-rails-code-optimization-and-cleanup
* https://medium.com/@kirill_shevch/lint-your-ruby-code-with-overcommit-and-static-analysis-tools-bd36d3147d2e
