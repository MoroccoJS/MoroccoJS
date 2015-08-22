title: How to install NodeJS using NVM on Ubuntu
author: Youssef Kababe
date: 2015-08-22 08:00:01
description: Step by step guide to install NodeJS on your Ubuntu machine using NVM (Node Version Manager)
category: tutorials
---

NodeJS is a platform that runs JavaScript on the server, allowing developers to build fast network applications quickly. In this tutorial I will show you how to install NodeJS on your {% link Ubuntu http://www.ubuntu.com/ true %} machine using {% link NVM https://github.com/creationix/nvm true "Node Version Manager" %}.

You may be wondering right now, why do we need to use NVM while we can install NodeJS from Ubuntu repositories using **apt-get**?

{% codeblock lang:bash %}
sudo apt-get update
sudo apt-get install nodejs
sudo apt-get install npm
{% endcodeblock %}

Well, yes that's true, you can install NodeJS easilly like this, but though it's correct it has some drawbacks! The first one, which had always annoyed me a lot, you'll always have to use **sudo** when working with NodeJS (Executing programs, installing packages...). Second one is you're stuck with the version available on Ubuntu repositories, you'll never get the latest version, and you'll have a hard time trying to switch to older versions as well.

NVM solves these problems by allowing you to install multiple, self-contained versions of NodeJS. Thus, you'll have total freedom choosing the version you want to use, and of course, you will never have to use **sudo** again!

First thing to do is the make sure we have **build-essential** and **libssl-dev** installed, NVM requires those two packages installed to build NodeJS.

{% codeblock lang:bash %}
sudo apt-get update
sudo apt-get install build-essential libssl-dev
{% endcodeblock %}

Now to install NVM, you simply need to execute the following command:

{% codeblock lang:bash %}
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.26.0/install.sh | bash
{% endcodeblock %}

{% asset_img nvm-install-command.png %}

Now just close your current terminal and open a new one, and you will be able to use NVM.

{% asset_img nvm-help.png %}

Typing NVM alone will display a help document that explains how to use it. Let's say we wan to list all available NodeJS versions:

{% codeblock lang:bash %}
nvm ls-remote
{% endcodeblock %}

{% asset_img nvm-ls-remote.png %}

If you want to know just the latest stable version instead:

{% codeblock lang:bash %}
nvm ls-remote stable
{% endcodeblock %}

Okay, now that we know what the latest version is, let's go ahead and install it!

{% codeblock lang:bash %}
nvm install v0.12.7
{% endcodeblock %}

Or:

{% codeblock lang:bash %}
nvm install 0.12.7
{% endcodeblock %}

Or simply:

{% codeblock lang:bash %}
nvm install stable
{% endcodeblock %}

{% asset_img node-installed.png %}

Want to install another version? Just do the same thing again and again and again... Now that you have multiple NodeJS versions installed on your machine, let's say you want to switch from **0.12.7** to **0.12.5**:

{% codeblock lang:bash %}
nvm use 0.12.5
{% endcodeblock %}

{% asset_img nvm-use.png %}

Too easy, huh? However, NVM will forget the NodeJS version you were using right when you close your terminal, and the next time you try to use NodeJS, you will be shown a cute shiny error telling you "command not found: node". That's because we didn't tell NVM which version to use by default. This can be easily done using aliases:

{% codeblock lang:bash %}
nvm alias default 0.12.7
{% endcodeblock %}

{% asset_img nvm-alias.png %}

That's it, you now have NodeJS installed with the default version set to **0.12.7**, next time you open your terminal NVM will automatically load this version for you.
