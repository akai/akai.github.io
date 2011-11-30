---
layout: post
title: "Installing Rails on  Mac OS X"
date: 2011-09-10 14:47
comments: true
categories: [Ruby, Mac]
---

<ol>
	<li>
		<p>Run software update</p>
	</li>
	<li>
		<p>Form Mac App Store download &amp; install <a href="http://itunes.apple.com/us/app/xcode/id448457090?mt=12">XCode</a></p>
	</li>
	<li>
		<p>Install Homebrew</p>
{% codeblock lang:bash %}
/usr/bin/ruby -e "$(curl -fsSL https://raw.github.com/gist/323731)"
{% endcodeblock %}
	</li>
	<li>
		<p>Install Git</p>
{% codeblock lang:bash %}
brew install git
{% endcodeblock %}
	</li>
	<li>
		<p>Install rbenv &amp; ruby-build</p>
{% codeblock lang:bash %}
brew install rbenv
brew install ruby-build
{% endcodeblock %}
	</li>
	<li>
		<p>Install Ruby</p>
{% codeblock lang:bash %}
rbenv install 1.9.2-p290
rbenv global 1.9.2-p290
{% endcodeblock %}
	</li>
	<li>
		<p>Install MySQL</p>
{% codeblock lang:bash %}
brew install mysql
{% endcodeblock %}
	</li>
	<li>
		<p>Install Rails</p>
{% codeblock lang:bash %}
gem install rails --no-rdoc --no-ri
{% endcodeblock %}
	</li>
	<li>
		<p>Enjoy!</p>
	</li>
</ol>

