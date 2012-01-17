---
layout: post
title: Microsites vs. Drupal with Foundation, part 1 - piece of cake?
permalink: /2012/01/16/microsites-vs-drupal-piece-of-cake
date: 2012-01-16 16:00
---

I've started building another microsite generator tool with Drupal. This meant I should start working on the Drupal version of Foundation LESS, too (being the quickest solution to theme a site for me at least).

I've run into a problem with theming however: I was looking for a way to simply add a class to every form but hook_form_alter() won't work in a template.php file. I've opened a [question at drupal.stackexchange.com](http://drupal.stackexchange.com/questions/19846/adding-a-css-class-to-every-form-in-drupal-6-in-a-theme) to see what's possible to do in this situation. The answer was pretty simple: [override ```theme_form()```](http://api.drupal.org/api/drupal/includes--form.inc/function/theme_form/6).

Also: switching jQuery version is a nightmare. Even with jquery_update you will have a hard time if you would like to use a recent jQuery (for example >=1.7) with Drupal 6. The best advice you can find in this area is [Using Newer Versions of jQuery with Drupal 6](http://drupal.org/node/1058168), which will tell you how to alias the newer version to use the old and the new jQuery side-by-side. I went with this, too; modifying 4-5 jQuery plugins (to play nice with noConflict) is way better than checking where Drupal breaks with the new version: is it Views, Panels or...? :)

