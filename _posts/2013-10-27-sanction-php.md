---
title: Sanction-PHP
description: Simple PHP role-base access control for Laravel 4 and others.
layout: post
published: false
tags:
  - Packagist
  - PHP
  - Packages
  - Laravel 4
---

I've spent more than a few hours in the past week looking at various access control packages for use with a few Laravel 4 applications i'm making (3 in total), unaware of the possible options, i opted to ask twitter, in order to skip the google search and get something that someone has already used and would reccommend:

<blockquote class="twitter-tweet"><p><a href="https://twitter.com/danmatthews">@danmatthews</a> <a href="https://twitter.com/semiBad">@semiBad</a> Zend Framework 2 has a newly rolled (see what I did there!?) ACL/RBAC lib available via Composer.</p>&mdash; Anthony Sterling (@anthonysterling) <a href="https://twitter.com/anthonysterling/statuses/393328252242370560">October 24, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet"><p><a href="https://twitter.com/danmatthews">@danmatthews</a> Sentry</p>&mdash; Zack Kitzmiller (@zackkitzmiller) <a href="https://twitter.com/zackkitzmiller/statuses/393334963581763584">October 24, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

So i took a look at all of these, and some more:

- [Sentry 2](http://docs.cartalyst.com/sentry-2)
- [Zend Framework Permissions & Acl](http://framework.zend.com/manual/2.1/en/modules/zend.permissions.acl.intro.html)
- [Authority & Authority L4](https://github.com/machuga/authority-l4)
- [Entrust](https://github.com/Zizaco/entrust)
- [Verify](http://docs.toddish.co.uk/verify-l4/#basic-usage)

All of these were great (and influenced Sanction heavily), but the one i really liked the most (Zend), which stored the permissions in memory, rather than in a database, stood out as stupid simple.

## What's different.

Although it uses `Zend\Permissions\Acl` under the hood to do the permissions matching, it actually does a lot for you:

- Roles and permissions are imported automatically from a config file (in Laravel 4). But can be passed in as an array for use with bare-bones PHP (and infact are, under the hood).
- Storing permissions against roles in a file, rather than a database, allows you to adjust and track them in source code, rather than the database.
- Inheritance is possible thanks to the Zend class automagically doing that work for us.
- It caches the result of building a permissions object for faster page loads (optional).
- It comes with an `Eloquent` trait that you can `use` in your `User` model to make querying roles and permissions against an instance of a user as easy as `$user->can('permission_name')`.
