---
layout: post
title:  "Add webfonts in your Rails asset pipeline"
date:   2017-09-25 11:53:43 +0200
categories: rails
---
Add your beloved fonts in `vendor/assets/fonts`

```ruby
#config/initializers/assets.rb
Rails.application.config.assets.paths << Rails.root.join('vendor', 'assets', 'fonts’)
Rails.application.config.assets.precompile << /\.(?:svg|eot|woff|ttf)$/
```

Add your **font stylesheet** at vendor/stylesheets/proximanova.scss (scss!) and use the font\_url helper!

```ruby
@font-face {
	font-family: 'Proximanova';
	src: font_url('proximanova-light-webfont.eot');
	src: font_url('proximanova-light-webfont.eot?iefix') format('eot'),
		   font_url('proximanova-light-webfont.woff') format('woff'),
		   font_url('proximanova-light-webfont.ttf') format('truetype'),
		   font_url('proximanova-light-webfont.svg#webfontFuT9mX6Y') format('svg');
	font-weight: 300;
	font-style: normal;
}
```

When you’ll run

**`rails assets:precompile RAILS_ENV=production`**

your font assets will be fingerprinted and copied inside public/assets, and font\_url knows how to get fingerprinted assets :-)
