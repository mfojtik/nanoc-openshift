# encoding: utf-8
source 'http://rubygems.org'

# Thin to serve content from Heroku
gem 'rack'
gem 'rack-rewrite', :require => 'rack-rewrite'
gem 'rack-contrib', :require => 'rack/contrib'

# Mime-types for handling mime types
gem 'mime-types', :require => 'mime/types'

group :development do
  # Nanoc for compiling dynamic code
  gem 'nanoc'

  # For spawning a file server in any directory and deploying to Heroku
  gem 'adsf'

  # HAML, Compass, Markdown and Builder for handling all important formats
  gem 'haml'

  # YUI Compressor to compress JS and CSS
  gem 'yui-compressor'
end
