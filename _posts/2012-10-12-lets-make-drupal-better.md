---
layout: default
title: Let's make Drupal better
description: I have a rant about Drupal, it's shortcomings, and what we can do to fix them.
---
Before i begin this post, let me preface it with the following:

1. This is my opinion, not that of Hydrant or my co-workers, that being said, i don't think anything here is controversial.
2. The things i'm suggesting below are big changes, yes, i know. But don't just dismiss them because they're 'too big', remember that these are proposed future changes, and remember to [Give it five minutes](http://37signals.com/svn/posts/3124-give-it-five-minutes).
3. Before you pull out the 'why don't you contribute to core' argument, please read ahead - i'm not a core contributor, i am a developer who **uses** drupal, and this post focuses on it from that point of view.

So in our recent team meeting, one of the agenda items was "Contribute more to drupal", as we make more of a wave in the industry, we also want to give things back - modules, fixes, tips, tricks and solutions. And we all agree that this is something we want to do - a lot of hard work goes into the modules we use daily, and we make money by using them, so it's only good karma to give something back.

Myself and [Chris](http://www.shoeboxdesign.co.uk) recently worked on a [module](http://drupal.org/sandbox/danmatthews/1728120) to integrate his support system into a drupal site, and saw that it would be a lot of use to others, so we submitted it to Drupal.org for review.

That was two weeks ago.

Not to mention that someone is sitting on the drupal project page with no commits, where as we have at least a minimum viable product.

My big problem is this - Drupal, while being open source, still feels like a closed ecosystem, we're not talking Apple-style "fenced garden" here, but it does feel remarkably closed compared to other open source projects.

I sit and complain about Drupal a lot, not because i don't like it, quite the opposite - i'm behind the idea that this is one of the most powerful tools available to people who build websites AND web applications (which we frequently do), and i want to help make it better, but contributing is difficult - i'm someone who *uses* Drupal every single day though, so surely my opinion is still valid?

So how about we change things up? Let's bring drupal into the present and radically change the way we're doing things, for the better.

## Documentation

Let me start here, because as someone who came into Drupal from basic PHP - i was building apps using PHP/MySQL but i'd never heard of the concept of a 'node', a 'hook', or what the hell a database abstraction layer was. As i've advanced i've noticed that sufficiently advanced modules really lack any documentation at all.

### For Core
Drupal's API documentation isn't terrible, but it could certainly stand to be better, even PHP's official documentation is easier to navigate, let's get ourselves a new, seperated, documentation system, let's start again, let's get back to basics and write a class reference that shows usage/implementation examples rather than the code of the function / class itself - a whole world more useful. And let's move it to:

    http://docs.drupal.org/core

### For modules
Oh god, modules - considering the constraint that Drupal puts on developers to create descriptive and helpful READMEs and documentation pages, the absolute number of projects i've seen with neither is enough to turn any new developer away from our lovely CMS.

Let's make it easy for people to contribute documentation for their project, including installation, usage, support and class references, and let's make it easy for them to find it, too:

If your project page is at:

    http://drupal.org/projects/mymodule

The documentation should be at:

    http://docs.drupal.org/projects/mymodule

 It's not rocket science, but it makes sense, and that's what matters.

## Github.

There is a mirrored [drupal repo on github](https://github.com/drupal/drupal) with the following blurb:

>Verbatim mirror of the git.drupal.org repository for Drupal core.
>Changes will not be pulled, if you want to contibute, go to Drupal.org

Er, what? Come on now... let's get Drupal on Github, let's make it so that anyone can fork it, make improvements, and submit pull requests for core. I'm not suggesting that we make it a free-for-all, but let's open it up completely - Drupal's locked-down source control isn't *that* much of a barrier, but it is somewhat of a hoop to jump through - reduce this to clicking 'Fork' in github, writing code, testing, then sending a pull request and numbers of contributors will jump.

Let's just look at the possible positives of moving drupal development to github:

* More contributors, no Drupal.org account needed, Github is largest community of developers.
* Tools are familiar to people who work with other open-source projects.
* Issue tracking is integrated with commits / pull requests.
* More feature and bug fixes submitted via the form -> pull request method.
* Better quality code, peer reviewed interactively on pull requests.
* More exposure - imagine the biggest PHP CMS moving it's development to github.
* Documentation (in github wikis) can be amended by users noticing errors by clicking "Fork", changing the mistake, then issuing a pull request.

## Bring development up to speed with current OOP paradigms.

Drupal is more than a site building tool to a lot of developers - we use it as an application framework instead of a traditional framework because it already has a lot of the admin tools there and ready to use.

I've heard whispers that Drupal 8 is going to be a development framework with the admin tool built on top - this would be brilliant.

PHP is (as of writing) at version 5.4 - bringing even more OOP goodness built on top of the fantastic features added in php5.3.

So why is drupal built and used so readily on legacy procedural code? I know there's a lot of OOP goodness in the backend, and that will improve with time, but why can't we use REAL OOP to do things like:

{% highlight php startinline %}
 $node = Node::load()->where('type', 'basic_page')->get(); 
{% endhighlight %}
Or:

{% highlight php startinline %}
 $node = Node::load(31);
{% endhighlight %}

Or to create a new save-ready node, just do this:

{% highlight php startinline %}

$node = Node::create(['type' => 'blog_post', 'title' => "Drupal is great"]);
{% endhighlight %}

Where the result is a fully loaded `instanceof Node`, allowing you to do lovely things like:

{% highlight php startinline %}

$node->title = "New Title";
$node->save(); // Saves
$node->delete(); // Deletes
{% endhighlight %}

Seriously, how nice would that be? The same could apply to Taxonomy objects:

{% highlight php startinline %}

$term = TaxonomyTerm::load(121, 31); // ( $term-id, $vocab-id );
$vocab = TaxonomyVocabulary::load(31);
{% endhighlight %}

And some lovely exceptions to handle the errors:

{% highlight php startinline %}

// Examples
try {
  $node = Node::load($node_id);
}
catch NodeNotFoundException($e) {
	DrupalMessage::set_error("Could not load the node with ID: {$node_id}");
}
catch NodeCannotBeLoadedException($e) {
  DrupalMessage::set_error("An unknown error occured, please contact support");
}
{% endhighlight %}

I must re-iterate that: **some of this, or similar, may already be possible, but it's not the way we're taught to do things**.

### Dependancies & Composer

Along those lines, let's change the way we get and deploy drupal sites, and involve something like [Composer](http://getcomposer.org/) and [Packagist](http://packagist.org/) - this is how the PHP community is passing dependancies around these days, and it's nothing short of fantastic - declare your dependencies once, and Composer will import them for you.

And making drupal module composer compliant? One `composer.json` file declaring it's information and we're done, maybe make drupal read these, instead of drupal `.info` files?

We have [Drush Make](http://drupal.org/project/drush_make), which is REALLY nice. So let's build on this - let's make this a thing, let's include a makefile for *all* drupal sites, and let's make it good practice to declare your modules in the makefile format, while we're at it, let's change the makefile format to use JSON - info files are incredibly outdated:

{% highlight json %}
{
  "drupal" {
    "core" : {
      "version" : "7",
      "release" : "latest"
    },
    "modules" : {
      "contrib" : {
        "views"
      }
    },
    "themes" : {
      "bartik",
      "seven"
    }
  }
}
{% endhighlight %}

How nice is that? Hit `drush make site.json` and drupal core is deployed with modules and themes. JSON is just, nicer, but that's just cosmetic - the real nicety here would be that we could create a drupal module that would keep this file in sync with the structure we have on already-deployed sites (especially if we're using Drush), and create this file for them, keeping more config in code, rather than in the database.

### Let's make drush a defacto utility.

One thing that i've noticed for drupal is that [Drush](http://drush.ws/) is a utility that just makes it **so much nicer** to work with from a developer standpoint, but the real shame is that it's not integrated as tightly as it could be.

For one, let's remove the need to 'install' it on your machine, let's have a drush binary (inaccessible from the web, of course, so that every download of Drupal core is just a `php drush <cmd> <arg>` away from all that powerful command line goodness. Full stack framworks are doing this now, and the inclusion of a command line utility to automate tasks is such a winner.

### Code & Config

One thing i'm tiring of is meetings where we sit around and discuss new and better ways to work, like working locally, then deploying code changes to staging servers via `git push`, but then what do we do with our database changes? There's `drush sql-sync` but it still doesn't solve the root problem.

How about proper migrations? And run them through `drush` or admin/config/migrations in the admin interface? The current schema API isn't that bad, it's the implementation if find difficult to swallow, if drupal took all the pain out of versioning SQL schema changes, we'd be in business - add a field in the interface, drupal creates a `DrupalSchemaMigration` object with `up` and `down` methods to track the changes and a nice description that you can see in the admin.

We might end up with 100s of small migration files, but being able to version control them is preferable to manually tracking features and the constant **NEEDS REVIEW** flag of doom.

## Enter symfony

I've never used [Symfony](http://symfony.com) personally, i tend to be more of a [FuelPHP](http://fuelphp.com) and [Laravel](http://laravel.com/) kinda guy, but i'm "Symfony Aware".

The blog post [here](http://symfony.com/blog/symfony2-meets-drupal-8) goes into depth about a lot of what i'm talking about (see the slideshow at the bottom for a great overview), but i still don't think it's going to be enough - they speak about freeing Drupal from ["Not invented here"](http://en.wikipedia.org/wiki/Not_invented_here) syndrome, but it feels to me like we may be forcing our developers into a new learning curve - Symfony itself.

## In conclusion

Okay so i've ranted a lot here - but my main points stay the same:

* Let's get Drupal on Github
* Let's get Drupal (even more) object oriented
* Let's turn drush into something easier to use and easier to install
* Let's try and reduce code/config so that the information required to MOVE a site's structure (not neccessarily it's content) is stored in code, and not in the database.
* Let's ramp up the development speed, making sure that updates are incremental and often, rather than the unwieldly large current ones (at least that way, upgrade paths are not required).

But, saying that, i think the most important thing for Drupal would be to open it up to the wider development community, get it out there and people looking directly at the code, arguing on pull requests about features, removing and simplifying things, and just generally talking about it more, it's a win-win for all of us, developers, Drupal based businesses, and users of the CMS alike.
