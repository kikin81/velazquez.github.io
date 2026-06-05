source "https://rubygems.org"

gem "jekyll", "~> 4.4"

# Site plugins
group :jekyll_plugins do
  gem "jekyll-remote-theme"   # loads remote_theme: mmistakes/minimal-mistakes
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-feed"
  gem "jemoji"
  gem "jekyll-include-cache"
end

# Local server (Ruby 3+) and Windows-only gems guarded by platform
gem "webrick"
gem "wdm", "~> 0.2.0", platforms: [:mingw, :x64_mingw, :mswin]
gem "tzinfo-data", platforms: [:mingw, :x64_mingw, :mswin, :jruby]
