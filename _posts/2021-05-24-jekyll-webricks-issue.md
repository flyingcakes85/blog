---
layout: post
title: Fixing Jekyll/Webrick Issue on Arch Linux
subtitle: Fix for version issue with ruby 3
categories: jekyll
banner: https://res.cloudinary.com/practicaldev/image/fetch/s--wUE4YoF7--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6tybpuxfd06olr6eiuge.png
tags: jekyll ruby tutorial
---

Jekyll may not run on Arch Linux because of an error with `webrick`. This is caused because webrick is not yet compatible with latest version of Ruby (3.0.1).

This is the relevant line from error output.

> /blog/vendor/bundle/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve/servlet.rb:3:in `require`: cannot load such file -- webrick (LoadError)

While setting up my GitHub pages blog, I faced this issue. I'll quickly document how to fix this.

### Install Bundle

You will need to have `rubygems` installed for this.

```sh
sudo pacman -S rubygems
```

Then install `bundle` via gem.

```sh
gem install bundle
```

Gem will print a warning about not having the local binary folder in your path. Add the following line to your `~/.bashrc` or `~/.zshrc` depending on the shell you use.

```
export PATH=/home/$USER/.local/share/gem/ruby/3.0.0/bin:$PATH
```

Make sure you replace "3.0.0" with the version number gem reports.

### Uninstall Ruby

We need to remove the latest Ruby version.

```sh
sudo pacman -Rns ruby rubygems
```

### Install Ruby 2.7

Webrick is compatible with Ruby 2.7. This is the version we will install.

```sh
sudo pacman -S ruby2.7
```

### Symlink `ruby` Binary

`bundle` will call the binary at `/usr/bin/ruby`. However, the old Ruby version is installed at `/usr/bin/ruby-2.7`. So we will create a symlink. You will need to uninstall `ruby` as I said earlier.

```sh
sudo ln -s /usr/bin/ruby-2.7 /usr/bin/ruby
```

Now, `/usr/bin/ruby` will just be a symlink (or shortcut in layman terms) to `/usr/bin/ruby-2.7`.

### Run `bundle`

Go to your Jekyll project directory. Install the dependencies.

```sh
bundle install
```

Then launch your Jekyll site

```sh
bundle exec jekyll serve
```

Your website should be now live! 🚀

You can receive latest updates from this blog as an RSS feed. See [Follow this blog via RSS](/blog/website/2021/05/23/follow-this-blog.html).

Follow me on GitHub : [flyingcakes85](https://github.com/flyingcakes85)
