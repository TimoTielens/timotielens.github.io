# frozen_string_literal: true

source "https://rubygems.org"

gem "jekyll-theme-chirpy", "~> 7.2", ">= 7.2.4"

gem "html-proofer", "~> 5.0", group: :test

gem 'jekyll-sitemap' #https://github.com/jekyll/jekyll-sitemap

group :jekyll_plugins do
  # ...
  gem "jekyll-securitytxt"
end

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.2.0", :platforms => [:mingw, :x64_mingw, :mswin]
