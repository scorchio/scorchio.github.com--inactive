---
layout: post
title: O Facebook, Facebook, wherefore art thou Facebook?
permalink: /2012/01/16/o-facebook-facebook-wherefore-art-thou-facebook
date: 2012-01-17 16:21
---
So here we go again, messing with the problem: why doesn't Facebook display the images from the site correctly when you like a post?

In my case, using Facebook's Object Debugger tool was not very helpful - it displayed one, seemingly random picked image and that's all.

I did my usual routine check on [StackOverflow](http://stackoverflow.com); maybe the best answer that I could found was [this](http://facebook.stackoverflow.com/questions/1138460/how-does-facebook-sharer-select-images#7623986), telling *almost* everything that I need.

Just the following three question of mine has remained:

1. Is there an actually usable rule on how Facebook selects pictures for sharing?
2. Can I force a picture for that?
3. Can I force multiple pictures with a sane markup?

At the end I decided to put a quick demo site together for playing around with several images that I would like to use on Facebook as a "like picture". Let's see what the Facebook Debugger tool said to that!

##How Facebook ranks pictures

Let's generate a set of images with a basic HTML5 markup and test with it.

It's really nice that my IDE supports Zen HTML: expanding stuff like ```ul>li*11>a[href="#"]>{<img src="$.jpg" alt="" />}``` is a breeze. (Also, you can see the not-so-strong points of it here, hence the "interesting" syntax for the image tag. I should be able to use images in a context like this *somehow* without inserting the HTML tag, but it would take even more time to figure out.)
    
As you can see from the Zen HTML snippet above, I'm trying to use several pictures - not only JPGs, I've modified the expanded code to include PNGs and GIFs too. (I've read somewhere that Facebook doesn't like PNGs so let's make that a part of the test.) Also the dimensions are varied in my selection. Images are located in one folder, named "images" (easy to figure out huh?)

###Images used

- 1\.jpg: 150x120, 3.72 Kb, aspect ratio: 1.33
- 2\.jpg: 150x120, 3.9 Kb, aspect ratio: 1.33
- 3\.jpg: 150x120, 4.28 Kb, aspect ratio: 1.33
- 4\.jpg: 150x120, 3.51 Kb, aspect ratio: 1.33
- 5\.jpg: 498x750, 223.77 Kb, aspect ratio: 0.664 
- 6\.gif: 200x200, 264.51 Kb, animated, aspect ratio: 1
- 7\.png: 150x111, 14.76 Kb, 32 bit color (transparent), aspect ratio: 1.35
- 8\.jpg: 165x165, 5.55 Kb, aspect ratio: 1
- 9\.jpg: 765x420, 27.43 Kb, aspect ratio: 1.8214
- 10\.jpg: 267x200, 8.41 Kb, aspect ratio: 1.335
- 11\.jpg: 765x420, 27.36 Kb, aspect ratio: 1.8214

order by width (desc): 9 & 11, 5, 10, 6, 8, 1-4 & 7

order by height (desc): 5, 9 & 11, 6 & 10, 8, 1-4, 7

order by file size (desc): 6, 5, 9, 11, 7, 10, 8, 3, 2, 1, 4

The debugger tool displays the ```og:updated_time``` value, which is the Unix timestamp of the last update. This way, I was able to check that it actually updated the information when it was needed.

###First test: list of images in links

I will try to figure out the sorting of the pictures with commenting out the actual picture selected by Facebook. My initial assumption is: Facebook tries to get the "best" picture, so it uses an internal ordering. As I comment out the top rated picture, Facebook selects another one. So the order is:
    
1. 8\.jpg
2. 7\.png (Oh, so Facebook actually does like the PNGs? It's transparent too...)
3. 9\.jpg (I'm starting to feel that this doesn't make any sense...)
4. 6\.gif (it doesn't matter that it's an animated GIF. Don't tell me...)
5. 10\.jpg
6. 5\.jpg
7. 11\.jpg
8. 4\.jpg
9. 3\.jpg
10. 2\.jpg
11. 1\.jpg

I've read that (see the linked StackOverflow answer) that Facebook checks the image aspect ratio. 

I don't know how well you can follow this, but at this point I'm clueless...

###Second test: image with link and list of image links

Let's modify the demo with moving one picture out of the ```<ul>```. I'm interested whether markup affects the sorting. I remove the previous top rated picture from to ul and place it in front of it, the order changes (I will strip the extensions, I'm sure you're smart enough to figure out what I mean):

1. 7
2. 6
3. 9
4. 5
5. 10
6. 4
7. 11
8. 3
9. 2
10. 1
11. 8

###Minor modifications on that second one: wrap the seperate one in a ```<p>```

Let me tell you that the previous top picture is without an explicit tag. Let's place it in a ```<p>```. The result of the sorting remains the same.

###...and again, reversed?

Move that ```<p>``` after the list. The sorting changes again!

1. 9
2. 7
3. 10
4. 6
5. 11
6. 5
7. 8 (Again: what?)
8. 4
9. 3
10. 2
11. 1

From this I get the feeling that Facebook **seems to rank images based on the tag context**, like a search engine checking for relevant content... but still, it's messed up. In the ```<ul>``` with ```<p>``` after it case, it seemed to "like" the middle, but I can't say really anything... Maybe that's because tag contexts are also weighted with seperate image ranking, but testing that is far beyond my current level.

##How to force a picture, then?

I would like to give you information not just about what works, but what fails, too. For example, using a ```<meta property="og:image" content="images/1.jpg"/>``` fails (most likely because the relative path). Using an absolute path instead works and the forced picture pops up.

As the StackOverflow answer mentioned above tells us, these kind of tags will break validation on the site, which could be solved by adding the necessary namespaces. Then what about HTML5? I'm quite interested on what [Validator.nu](http://html5.validator.nu) says about this... Of course, it fails without the namespaces. But with adding the recommended namespaces as Facebook recommends it fails even harder, number of errors jumping from 2 to 6 :D I assume we will have a solution for that as soon HTML5 becomes the main standard everywhere.

##Let's try forcing multiple pictures!

Well this part of my testing was easy and successful, unlike other parts of the test: more ```<meta property="og:image" ... />``` you add, more picture pops up. Simple. Even better news: the pictures aren't sorted any way, they maintain the order in the source.

##What about the old way?

There was (once) a markup like ```<link rel="image_src" href="http://yoururl/yourimage"/>``` which would do the same as the ```<meta>``` above. It still works... to a degree. If you use one tag mentioning an image, it will be used as the like picture. However if you have more links like that, only one of those is used. (Which is a shame, it would be nice if we could pull more images in with this technique.)

##End of searching for a viable, sane markup

I did this testing because I wanted to find out the real way on implementing this with the CMSs I currently use (Drupal and WordPress). After studying this, my suggestions are the following for anyone who looks for advice:

- Supply a default ```<meta property="og:image" ... />``` for your site; make that the first og:image in the source, so it will appear first when the user selects the like picture. The logo of the site could be a nice choice.
- For pages displaying multiple articles (such as a main page on a blog), supply alternative ```<meta property="og:image" ... />``` tags for every article popping up on the actual page. WordPress has a built-in way to assign pictures to each post, named "Featured Image"; with Drupal it's a bit harder, you would have to modify your content types to incorporate an image field etc., but at the end it's almost the same.
- For pages displaying a single page/article, use only the default and the featured image.

As far as I can understand the way Facebook works at the moment, this could be the right method to make those pictures available in a sane way.

###WordPress solution?

Check [this article about how to implement Open Graph tags in a theme](http://www.wpbeginner.com/wp-themes/how-to-add-facebook-open-graph-meta-data-in-wordpress-themes/). It doesn't try to implement the single page/article case as I suggested, however it has almost everything which could make it possible to implement.

###...and the Drupal 6 way

I'm happy to say that as far as it seems we have well-thought Drupal module just for this: [Open Graph meta tags](http://drupal.org/project/opengraph_meta). As the project page tells us:
  
> This module makes it easy to select the image thumbnail used to represent the node (used by Facebook when constructing a preview). The editor is shown a list of thumbnails of all images   associated with the node (both as fields as well as images embedded within the node's body   content).

####Edit:

*I've just played a little bit with Nodewords. As the main solution for adding meta tags for a Drupal site, it has support for Open Graph tags, too - very nice addition to have.*

###Edit - what about Drupal 7?

Oh WOW! The Nodewords module was completely rewritten for Drupal 7, it's called [Meta tags](http://drupal.org/project/metatag) and supports all Open Graph tags in a really elegant, nice way... Just check out this beauty:
<img src="http://drupal.org/files/images/Meta%20tags%20%7C%20Drupal%207%20test%20site.preview.png" alt="Drupal 7 with Meta Tags module" />

So that's all for today, folks. This misery really tired me out, but still, I've learned a lot. If you have any additional information, please share with me in the comments, I would love to know more about this!

####P.S. Solution == Un-solution?

Here we have a philosophical side too...

Open Graph tags are not really meant to use on a page which contains a bunch of different content. 

> It is currently designed for Web pages representing profiles of real-world things — things like movies, sports teams, celebrities, and restaurants.

For articles, it's okay; videos too, etc., but what about a front page? What should be the main image associated with a site's front page in the Open Graph philosophy? How should fall back to basic site informations if we don't have different content types? What should we do with a page which contains a random number of different content? In my opinion, the picture above - with Meta tags and Drupal 7 - shows how it should be done - can you spot any weakness there?

Share your ideas, let's start a discussion!