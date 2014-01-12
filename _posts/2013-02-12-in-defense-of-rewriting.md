---
description: Why rewrite something from scratch? To improve and learn.
title: In defense of rewriting.
layout: post
---

There's this kind of running joke about me and http://simplysatisfied.net at the moment, that as soon as i find 'a new framework' to re-write it in, i do.

Why? Because i wrote it originally in CodeIgniter, this was back when it was the *only* framwork i knew of, every call was routed through the `$this` variable (`$this->db->query()` etc), then i found out what Kohana was, although i never switched, and through some work stuff i played with Zend Framework, and even started to try and learn it, but it seemed horrific to learn: simple things seemed madly overcomplicated. In the meantime, i shelved the app's development while i improved my Drupal skills for my day job.

Until i found [FuelPHP](http://fuelphp.com), that is. Static method calls seemed to be an amazingly concise way to write things compared to the Codeigniter way, and i was totally taken aback - it was the first full stack framwork i felt i could pick up, and i loved it, so i rewrote a fairly youthful web app in FuelPHP.

Now? It's working, running great, on the verge of being pushed live to the wider public along with payment processing, and using a few composer packages too.

## Ruby ruby ruby ruby.

All i saw all day every day were more and more arguments about how Ruby was lovely to work with, better than PHP, and i even noticed how a good few Ruby jobs paid more than PHP ones, so i decided to take a look.

I liked it, i like the syntax, the idea behind rails and the possibilities - all along knowing i didn't have enough knowledge of ruby to fully appreciate the things i would miss when i went back to PHP. The idea of rebuilding something in rails wasn't so much about using 'what was cool' as it was i was addicted to learning something new.

It looks like i dodged a bullet not building it in rails anyway, judging by all the latest security releases.

## PHP -> Composer -> Symfony -> Laravel

Somewhere on a journey into learning what PHP 5.3's OOP power could REALLY do (before finding FuelPHP, i didn't even understand things like Closures were), i found [Composer](http://getcomposer.org)  - what i came to understand to be the equivalent of Ruby's Gem, and Node's NPM (or the closest we could get), i started to learn how to download packages with it, then i learned how to write packages for it.

From there, i found Symfony. Most of the best, well tested packages i found on [Packagist](http://packagist.org) were symfony components AND because Drupal 8 is supposedly using a good few symfony components. Symfony is great, but it felt overcomplicated (In the same vain as Zend, also please bear in mind i was still a full-on OOP n00b at this point).

And from there, somehow, i found a blogpost about [Laravel 4](http://four.laravel.com), and how the upcoming version of this framework i'd never heard of would use Composer to manage the entire framework, it sounded like the future, but the future was still quite far away in terms of the status of the framework itself.

I looked at Laravel 3, and in many many ways it was the same as FuelPHP - Migrations, A CLI tool, tasks, lots of helper classes, but the syntax looked even more beautiful than Fuel's, i resisted temptation for a long time to re-write my app in Laravel 3 for what would essentially be a pointless change for the end user.


## Skip to the end.

To this day, [Simply](http://simplysatisfied.net) is a FuelPHP application, which is not a bad thing, but i am currently writing the REST API for it in Laravel 4 - for two reasons:

1. Resourceful controllers, a part of L4, are the exact thing that i want and need for my API.
2. Eloquent, the ORM in L4, is really lovely to use, and powerful.
3. I want to learn something new, so shoot me!

Eventually, i envisage moving a lot of the application to Laravel 4 to take advantage of some of the awesome new features like built in Queues support and better more elegant support for some of the things that FuelPHP does great, but Laravel does better.

## Don't chase the rabbit

Trying to keep up is an endless chase, the technology your app is written in is often insignificant by the time it hits a production server (especially for hobbyist app developers like myself), but you can't keep changing what you use to use "what's cool" or relavant at the time.

Sensible change, and slow (testable) evolution rather than revolution is seemingly the key to making something that you can ship without driving yourself insane.
