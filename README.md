# UC Davis DSS Developers Blog

Blog for the DSS IT developers at UC Davis.

## Installation

git clone git@github.com:dssit/developers.dss.ucdavis.edu.git
cd developers.dss.ucdavis.edu.git
bundle install
bundle exec rake preview

## Creating a post

To create a new post, all you need to do is create a new file in the _posts directory. How you name files in this folder is important. Jekyll requires blog post files to be named according to the following format:

YEAR-MONTH-DAY-title.MARKUP

Check the example post


## Front Matter

The post header includes the following definitions, of which, some are theme specific:

layout: [post / page]
title:  [String (no need for quotations)]
date:   yyyy-mm-dd
description: [String (no need for quotations)]
categories:
- list
- of
- categories
permalink: could-be-the-title-of-the-post
author: [chris / obada / lloyd]
banner_image: /relative/to/assets/images/
banner_video: '<iframe width="560" height="315" src="//www.youtube.com/embed/pU9Q6oiQNd0" frameborder="0" allowfullscreen></iframe>'

More info: http://jekyllrb.com/docs/frontmatter/



## Post content

More information about markdown:
http://jekyllrb.com/docs/posts/
https://guides.github.com/features/mastering-markdown/


Example posts in the theme package:
https://github.com/gayanvirajith/gaya/tree/master/_posts


## Publishing

Once you create a new post, commit changes to master
then run
bundle exec jgd

This will copy the content of _site directory to the gh-pages branch and push it.
