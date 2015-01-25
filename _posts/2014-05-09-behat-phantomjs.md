---
title: Using Behat with PhantomJS
layout: post
---

I've searched low and high for the instructions to get PhantomJS working with my current Selenium/Firefox/Behat acceptance testing setup at work - the existing documentation is confusing (in my opinion) and led to many extra hours in the office trying to tie the two together.

Here's the good news: it's really damn simple when you figure it out, or in the case of this blog post (hopefully) when someone explains it, step by step.

Essentially, the missing step is that **we're only pretending to use Selenium Server**.

PhantomJS has this awesome thing built into the recent versions called GhostDriver, it's essentially a little WebDriver that will automate the testing for you, much like Selenium does for real browsers like firefox.

The missing step in comprehension for me came when looking at articles that used PhantomJS _through_ Selenium Grid, using Selenium as the webdriver, but instead, you can bypass selenium altogether and just use Phantom and GhostDriver.

### Install PhantomJS (and GhostDriver)

PhantomJS runs using NodeJS, so you'll need to have that, and it's package manager, `npm`, installed.

Installing phantomjs is easy:

{% highlight bash %}
$ npm install phantomjs
{% endhighlight %}

### Update your Behat.yml config.

{% highlight yaml %}
phantomjs:
  extensions:
    Behat\MinkExtension\Extension:
      base_url: 'http://mysite.co.uk'
      goutte: ~
      selenium2:
        wd_host: "http://localhost:8643/wd/hub"
{% endhighlight %}

Take note of the port (`:8643`) you set, you'll need to set this port number for PhantomJS next.

### Start GhostDriver

Start GhostDriver on the port listed in your config, which for us was `8643`:

```
$ phantomjs --webdriver=8643
```

### Run Behat!

```
$ ./bin/behat -p phantomjs
```

This will run your tests using the `phantomjs` profile instead of your default one, all being well, it should use Behat's Selenium2 driver, and communicate successfully with GhostDriver.

