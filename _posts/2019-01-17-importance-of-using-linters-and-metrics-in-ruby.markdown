---
layout:     post
title:      "Importance of Using Linters and Metrics in Ruby"
date:       2019-01-17 07:52:00 +0000
permalink:  'importance-of-using-linters-and-metrics-in-ruby'
---

Ruby is a dynamic, expressive and flexible language. To achieve [programmer happiness](https://rubyonrails.org/doctrine/#optimize-for-programmer-happiness) Ruby has a large standard library. And, if you use Rails, the standard library is even bigger. There are many ways to do things with Ruby. This is often what draws programmers to it in the first place.

However, as a project grows in size and complexity, more structure is required in order to manage it. Structure must be added at multiple levels of abstraction: syntax, testing, project structure as well as framework and language usage. Most of these facets can be improved by tooling.

### Syntax

Does the project allow trailing whitespace? Does it use `self::some_method`, `self.some_method` or both for class method definition? Does it allow method params to be defined without parentheses?

In isolation these decisions are trivial, which is why you should set a standard and stick to it throughout the codebase. Inconsistent syntax makes code hard to read and causes pointless arguments in code reviews.

To avoid all this, install [Rubocop](https://github.com/rubocop-hq/rubocop) and have it run during CI. Pick a sensible set of styles or stick with [Rubocop's default configuration](https://github.com/rubocop-hq/ruby-style-guide).

### Testing

Code coverage tools should be used to maintain high code coverage. Whatever your desired coverage (why not 100%?) you should use a tool to ensure that coverage doesn't drop. Use something like [SimpleCov](https://github.com/colszowka/simplecov) and add it to CI. If you're using SimpleCov, I recommend setting the `SimpleCov.refuse_coverage_drop` option to prevent PRs from lowering coverage. That way, coverage will increase overtime.

### Project structure

Consistent project structure is very important. For example, if a project uses service objects you don't want new developers adding logic to the models. Or if you implement the [strategy pattern](https://www.amazon.co.uk/gp/product/0321490452/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=kadwill-21&creative=6738&linkCode=as2&creativeASIN=0321490452&linkId=354f3091ecc14e51812a21787dc09af0), future developers should continue using the pattern when adding new behaviour. Unfortunately, these things are difficult to enforce automatically. But, there may be tools out there that I haven't discovered - please tell me if you know any.

The best way to maintain a good and consistent project structure is through pull request reviews and pair programming.

### Framework and language usage

Lastly, there are tools like [rails_best_practises](https://github.com/flyerhzm/rails_best_practices) that give you language and framework suggestions. rails_best_practises won't improve high level design but it will find law of demeter violations or tell you not to use default scopes. I find rails_best_practises invaluable for writing clean and performant rails code.

### Summary

Make use of all the great Ruby community tools. Add them into CI so that PRs all follow the same high standards.
