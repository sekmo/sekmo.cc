---
layout: post
title:  "Add webfonts in your Rails asset pipeline"
date:   2017-09-28 11:53:43 +0200
categories: rails
---
Some months ago I spent a couple of hours trying to get a webfont working with the Rails asset pipeline.
I thought it was worth writing a turboguide on how to make it work without getting a headache.

1) Copy your beloved fonts in `vendor/assets/fonts`

2) Add the next lines at the end of `config/initializers/assets.rb`

```ruby
# ...
Rails.application.config.assets.paths << Rails.root.join('vendor', 'assets', 'fonts')
Rails.application.config.assets.precompile << /\.(?:svg|eot|woff|ttf)$/
```

3) Copy your font stylesheet to `vendor/stylesheets/merriweather.scss` and use the `font_url`
sprocket helper to get the font location.

```ruby
@font-face {
  font-family: 'merriweather';
  src: font_url('merriweather-webfont.eot');
  src: font_url('merriweather-webfont.eot?iefix') format('eot'),
    font_url('merriweather-webfont.woff') format('woff'),
    font_url('merriweather-webfont.ttf') format('truetype'),
    font_url('merriweather-webfont.svg#webfontFuT9mX6Y') format('svg');
  font-weight: 300;
  font-style: normal;
}
```

4) Run the command **`rails assets:precompile RAILS_ENV=production`**

After issuing this command your font assets will be fingerprinted and copied to `public/assets`, and `font_url` knows how to get fingerprinted assets :-)
