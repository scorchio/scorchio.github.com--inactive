---
layout: post
title: Make a site form me, quickly!
permalink: /2012/01/18/make-a-site-for-me-quickly.markdown
date: 2012-01-18 15:00
published: false
---
So today is the day. I didn't plan to work for very long today (after all, I'm fighting sleep deprivation and lack of social activities), but it's still okay for playing around a little with ```drush make``` and friends.

In my earlier projects starting Drupal sites involved quite a bit of manual work. Downloading tarballs, extracting, setting up the database for the new site, etc. etc. With ```drush``` it's easy; with ```drush make``` it's fun!

Someone at [Four Kitchens](http://fourkitchens.com/) had a great idea: now we have ```drush make``` let's build a makefile generator. And they did, so we have a great and easy to use tool to generate drop-in make configurations with the most popular and useful Drupal modules around. And we have it for 6 & 7, too! You can find this tool at [drushmake.me](http://drushmake.me).

After the make phase, I start installing the site with ```drush site-install```. This might be the one of the longest CLI command I've typed in but as you will see, it saves me from a lot of stuff to do...

     drush site-install --db-url=mysqli://<db-username>:<db-pass>@localhost/<database> --locale=hu --account-name=<name of user 1> --account-pass=<password of user 1> --account-mail=<e-mail address of user 1> --site-name=<name of the site> --site-mail=<e-mail address of the site> --db-su=<database administrator username> --db-su-pw=<database administrator password>

Yes, I'm installing a Drupal site with Hungarian locale right now, that's why the ```--locale=hu```. On my development machine this site-install thingy took 7 seconds of my life, precious time... So I won't worry if something is missing for the first time. I tend to forget to copy over the files needed for the Hungarian locale, or patching .htaccess (for running on my local setup a one-line modification is needed).

Well, it didn't work on first try; it seems to dislike when I specify locale. I've installed Drupal 6 without the locale and after ```drush en locale``` I've selected Hungarian, that's all.

There are so many good Drupal modules that you might be tempted to switch on everything... well, that would be a stupid idea, unless you would like to make the slowest site in the history of mankind. See, every module requires maintenance - more you install, more careful you have to be with upgrading.

Well it turns out the site I'm going to build is my first Drupal 7 site that will go into production. Wow, I'm shocked.

Okay, so after making my little tools D7-friendly, here we go with the first install. I remember that once I've tried installing with Hungarian locale and it was a bit problematic, but I still want to re-check that... Well yeah, the installer got stuck at importing localizations. Gábor Hojtsy comes to our rescue with the friendly [Localized Drupal Distribution](http://drupal.org/project/l10n_install) project. With using the localized profile, there's no problem, oh yeah. This install takes quite a bit of time, so feel free to grab a coffee or a tea while waiting :)

The modules I've installed:
- Token (needed for Pathauto)
- Pathauto
- Transliteration
- Libraries
- Wysiwyg
- IMCE
- IMCE for Wysiwyg

Quite interesting that for this project I've stopped using Wysiwyg + CKeditor and instead started using the CKeditor module. In Drupal 7 the input formats are better, too, so it seems I won't need the Better Formats module anymore.

Boron is an HTML5 base theme for Drupal 7. I've downloaded this to check out and use it for a starter theme (powered up with Foundation-LESS stuff of course ;) ). Good thing: my old fav, the Rubik administration theme is available for D7 and it's working well. (Don't forget to download it's base theme, Tao, if you would like to test it.)

I've installed jquery_update too. It's still something that should be done if you would like to use a recent jQuery.

Let's see what's up with meta tags! Nodewords has been rewritten for Drupal 7 and now it's called Metatag.f