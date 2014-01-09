# tracehelms.com
Yep, that's right. This is my website, it's internals all open for display.

Find it live at <http://tracehelms.com>.

### Getting It Working

#### Building The Site
Run `rake build`.

#### Serving The Site Locally
Run `jekyll serve`. Browse to `http://localhost:4000`.

#### Deploying
Install and configure [Amazon CLI](http://aws.amazon.com/cli/) first.
Replace the `BUCKET_NAME` constant in Rakefile with your bucket name.
Then run `rake deploy` and watch the magic happen!

### Future Plans For This Site
Failing to plan is planning to fail!  Here's what to expect:

* Add a generator for new posts and pages
* Add a page linking to code projects
* Add a page for recent GitHub activity (pulled from GitHub)
* Try to minimalize the style
* Add Disqus comments

### Technologies (thanks!)

* [Jekyll](http://jekyllrb.com)
* [SASS](http://sass-lang.com)
* [The Semantic Grid System](http://semantic.gs)
* [Amazon S3](http://aws.amazon.com/s3/)
