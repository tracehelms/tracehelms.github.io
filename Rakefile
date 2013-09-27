require 'fileutils'
# Compiles the SCSS stylesheet and builds the site

# Replace this with your bucket name.
BUCKET_NAME = 'tracehelms.com'

task :build do
  build
end

# Builds the site with Jekyll and then deploys to the S3 bucket.
# Existing site is backed up in the _backup directory on S3.
task :deploy do
  build
  system "aws s3 rm s3://#{BUCKET_NAME}/_backup/ --recursive"
  system "aws s3 mv s3://#{BUCKET_NAME}/ s3://#{BUCKET_NAME}/_backup/ --recursive"
  system "aws s3 sync _site s3://#{BUCKET_NAME} --exclude *_backup/*"
end

# Compiles SASS files and then compiles the site.
# TODO Add SASS generation to Jekyll itself instead of running it in a rake task here.
def build
  system 'jekyll build'
end
