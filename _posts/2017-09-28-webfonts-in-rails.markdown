---
layout: post
title:  "Add webfonts to the Rails Asset Pipeline"
date:   2017-09-28 11:53:43 +0200
categories: rails
permalink: notes/add-webfonts-to-rails-asset-pipeline
---
Some time ago I blissfully spent a couple of hours trying to get a webfont working with the Rails' Asset Pipeline.
I thought it was worth writing a quick guide on how to make it work without getting a headache :-)

First copy your fonts in the directory `vendor/assets/fonts`, then add the next lines at the end of `config/initializers/assets.rb`:

```ruby
# ...
Rails.application.config.assets.paths << Rails.root.join('vendor', 'assets', 'fonts')
Rails.application.config.assets.precompile += %w[.svg .eot .woff .ttf]
```
The first line tells the Asset Pipeline to use the `font` directory as an additional source for assets, and the
second one specifies which kind of files should also be taken in consideration when compiling the assets.

Now create your font stylesheet at `vendor/stylesheets/merriweather.scss`, and use the
sprocket helper `font_url` to automatically get the font location:

```ruby
@font-face {
  font-family: 'merriweather';
  src: font_url('merriweather-webfont.eot');
  src: font_url('merriweather-webfont.eot?iefix') format('eot'),
    font_url('merriweather-webfont.woff') format('woff'),
    font_url('merriweather-webfont.ttf') format('truetype'),
    font_url('merriweather-webfont.svg#svginfoUuR4hF7w') format('svg');
  font-weight: 300;
  font-style: normal;
}
```

In the development environment the asset files are just preprocessed (e.g. scss to css) and
[fingerprinted][1]{:target="_blank"} (adding the digest to the filename to prevent caching), and they
should work without any additional steps.

If you're manually deploying your rails app you'll need to run the following command:
```bash
rails assets:precompile RAILS_ENV=production
```
The asset files will get preprocessed, minified, concatenated (better to have a single css file to have less HTTP
requests), fingerprinted, and copied to `/public/assets`.

Whether we're on the development or production environment, the `font_url` helper will always return the right
path for the compiled font assets.


[1]: http://guides.rubyonrails.org/asset_pipeline.html#what-is-fingerprinting-and-why-should-i-care-questionmark
