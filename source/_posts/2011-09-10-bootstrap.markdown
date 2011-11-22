---
layout: post
title: "Installing Rails on  Mac OS X"
date: 2011-09-10 14:47
comments: true
categories: [Ruby, Mac]
---

1. Run software update

2. Form Mac App Store download & install [XCode](http://itunes.apple.com/us/app/xcode/id448457090?mt=12)

3. Install Homebrew

		/usr/bin/ruby -e "$(curl -fsSL https://raw.github.com/gist/323731)"

4. Install Git
		
		brew install git

5. Install rbenv & ruby-build

		brew install rbenv
		brew install ruby-build

6. Install Ruby

		rbenv install 1.9.2-p290
		rbenv global 1.9.2-p290

7. Install MySQL

		brew install mysql

8. Install Rails

		gem install rails --no-rdoc --no-ri

9. Enjoy!



