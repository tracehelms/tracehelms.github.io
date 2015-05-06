---
layout: post
title:  Coming Along
date:   2014-01-11 14:44:00
categories: posts
---

Ahhh, a functioning website. One that doesn't look completely terrible even! (OK, don't crush
my hopes and dreams just yet.) As detailed in the last post, I recently converted my site
to use [Jekyll](http://jekyllrb.com/) instead of [WordPress](http://wordpress.org/).

I've heard WordPress get a little bit of hate from a couple of programmers I've talked to,
which at times can be understandable. Mostly, though, WordPress is awesome. For me, it was a
little too heavy. I don't really need the database for anything. I'm writing the code from
scratch (mostly to learn). Plus, static sites are WAY cheaper to host (thanks
[Amazon S3](http://aws.amazon.com/s3/)).

## How's It Done?
Jekyll is a Ruby-based blog that generates static HTML for you to upload to your server.
"Templates" are written in HTML and each blog post is written in
[Markdown](http://daringfireball.net/projects/markdown/). Since the entire site is plain 'ole
HTML, you can host it just about anywhere. I have found that Amazon S3 is very cheap, and I
pay about $0.50 per month to host this site.

Having to deploy the entire site each time is a bit of a hassle, but I've written a nice little
Rake task to do it for me.

{% highlight ruby %}
# Replace this with your bucket name.
BUCKET_NAME = 'tracehelms.com'

# Builds the site with Jekyll and then deploys to the S3 bucket.
# Existing site is backed up in the _backup directory on S3.
task :deploy do
  system 'jekyll build'
  system "aws s3 rm s3://#{BUCKET_NAME}/_backup/ --recursive"
  system "aws s3 mv s3://#{BUCKET_NAME}/ s3://#{BUCKET_NAME}/_backup/ --recursive"
  system "aws s3 sync _site s3://#{BUCKET_NAME} --exclude *_backup/*"
end
{% endhighlight %}

Now I can simply type `rake deploy` and the site gets pushed live. The old version of the site
is even backed up!
