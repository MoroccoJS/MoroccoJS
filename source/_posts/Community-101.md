title: Community 101
description: "Introducing MoroccoJS and contribution guide"
author: Youssef Kababe
date: 2015-08-11 23:43:44
category: articles
---

If you've been following the evolution of {% link JavaScript https://www.javascript.com/ true %} for the past couple of years, then you surely have noticed how fast it's moving forward and how much stronger it has become. From a simple browser scripting language to manipulate the {% link DOM https://en.wikipedia.org/wiki/Document_Object_Model true "Document Object Model" %}, to a language that can be used to power an entire Web application on both the front-end and the back-end, JavaScript's future really seems promising!

However, because of how easy it may look at first, most of the developers who use JavaScript don't invest time in learning the language, they just go ahead and use it. It's true that you can use JavaScript and any framework or library based on it without having a solid knowledge of the language but, besides never knowing what you're doing, you will barely scratch the surface of what JavaScript can do.

We started **MoroccoJS** in the hope of building a strong Moroccan community around JavaScript, a community where every member is eager to learn and share his knowledge with the world, and to make people aware of what they're missing. One of the things I noticed about developers from other countries is that they don't only consume what others create, they instead learn, share what they have learned, and also create more amazing stuff on top of that! Isn't that the way how technology advances? Indeed it is, and that's why one of the goals of our community is to change the mentality of the Moroccan developer from a dumb consumer to a smart contributor and producer.

## Website structure

As you can see from the navigation bar, the website is divided into multiple sections or categories:
- **Articles**: General JavaScript articles.
- **Tutorials**: Libraries, frameworks, and language tutorials.
- **News**: Latest news from the JavaScript world.
- **Events**: JavaScript meetups, conferences, and events.
- **Jobs**: Job offers and opportunities.

I think that's pretty clear and straightforward, nothing to explain about it. Now I know you can't wait to start writing but, there is more stuff you need to understand. **MoroccoJS** is built with {% link Hexo https://hexo.io true %}, a JavaScript static website generator, and hosted on {% link GitHub https://github.com true %}. I will not cover everything about Hexo and GitHub in this article, maybe in upcoming tutorials, but I will instead point you to the things you need to know in order to start writing.

## How to contribute

First things first, make sure you have {% link Git https://git-scm.com/ true %} installed on your system. If you have no prior knowledge about Git and GitHub, please visit the website and learn, you may find useful information on {% link "GitHub Help" https://help.github.com/ true %} too. The next thing to do is to fork the website's {% link repository https://github.com/MoroccoJS/MoroccoJS true %} on GitHub.

{% asset_img mjs-repo.png %}

Forked the repository? Alright, you now have your own copy of **MoroccoJS** that you can clone to your computer. Please don't forget to link the original repository to your local one as remote, so you can easily keep it up to date when something new was pushed. Again if you don't understand what I'm saying here, please take some time to learn Git!

As I said earlier, this website is built with Hexo, a good static website generator written in JavaScript and runs on {% link NodeJS https://nodejs.org true %}. Luckily, we have a tutorial for you on how to install NodeJS on your computer {% post_link Installing-NodeJS-using-NVM-on-Linux here %}! Then next step after having NodeJS installed is to install Hexo and {% link Bower http://bower.io true %}, we use Bower to manage front-end dependencies. You can do that by typing the following lines, each one at a time, in your terminal:

{% codeblock lang:bash %}
npm install hexo-cli -g
npm install bower -g
{% endcodeblock %}

After that you need to install development dependencies, you can do it like this:

{% codeblock lang:bash %}
npm install
bower install
{% endcodeblock %}

Congratulations, you've just passed the hard part! You can now go ahead and launch the Hexo server using the following command:

{% codeblock lang:bash %}
hexo server
{% endcodeblock %}

Then open http://localhost:4000 in your browser and you'll be able to see an identical copy of **MoroccoJS** running locally on your computer. Now don't let that huge file tree fool you, you'll be spending most of your time inside the ***source/_posts*** folder because that's where all the posts live. One last thing before writing your first post, you need to add yourself to the ***source/_data/authors.yml*** like so:

{% codeblock "MoroccoJS/source/\_data/authors" lang:html %}
Youssef Kababe:
  email: hello@youssefkababe.com
  github: YoussefKababe

Your Name:
  email: your.name@example.com
  github: your_github_username
{% endcodeblock %}

Alright, we're good to go now! Hexo has a nice way to create new posts using the command line, let's say you want to create a new post called "Introduction to JavaScript", all you need to do is to head to your terminal and type the following command:

{% codeblock lang:bash %}
hexo new post "Introduction to JavaScript"
{% endcodeblock %}

Then you'll find your post under the ***source/_posts*** folder like so:

{% asset_img file-tree.png %}

If you look closely, you'll see that Hexo has created a folder with the same name of your new post as well, well that's just Hexo being smart, he knows you may want to include pictures in your post so he created a folder for you to put them in! The first thing you're going to put in that folder is a 200 x 200 px thumbnail that describes what your post is about, try to be creative and save it under "thumbnail.png". Hexo has also put something in the top of your post:

{% codeblock "MoroccoJS/source/\_posts/Introduction-to-JavaScript.md" lang:html %}
title: Introduction to JavaScript
date: 2015-08-22 01:56:46
description:
category:
author:
---
{% endcodeblock %}

This is called the **Front-matter**! It's just a little block that contains some information about your post. Once you're done with that, you can then start writing your post's content in {% link Markdown http://daringfireball.net/projects/markdown/syntax true %}. If you want to include external content like images from the Internet or Youtube videos, please refer to Hexo's {% link "Tag Plugins" https://hexo.io/docs/tag-plugins.html true %} documentation.

I think we've come to the end of our community introduction, all you have left to do now is to commit your new post and push it to GitHub, then send us a pull request to let us know about your work. We hope we will always work towards building a brighter future and a great Moroccan JavaScript community together!
