# tracehelms.com
Yep, that's right. This is my website, it's internals all open for display.

Find it live at <http://tracehelms.com>.

## Getting It Working

### Building The Site
Run `rake build`.

### Serving The Site Locally
Run `jekyll serve`. Browse to `http://localhost:4000`.

### Deploying
Install and configure [Amazon CLI](http://aws.amazon.com/cli/) first.
Replace the `BUCKET_NAME` constant in Rakefile with your bucket name.
Then run `rake deploy` and watch the magic happen!

## Future Ideas For This Site
* Add a page for recent GitHub activity (pulled from GitHub)
* Add Disqus comments

## Technologies (thanks!)

* [Jekyll](http://jekyllrb.com)
* [Amazon S3](http://aws.amazon.com/s3/)
