title: npm based front-end workflow
date: 2015-09-17 13:30:00
description: A tutorial on how to use npm to easily manage your front-end dependencies and compile your assets
category: tutorials
author: Youssef Kababe
---

Long gone are the days where developers used to download dependencies manually, place them in their project folder, and include them in their web page using multiple **script** and **link** tags. The fast evolution of web development gave birth to a wide variety of tools that help developers speed up their workflow, write clean code, automate repetitive tasks, and maintain maximum performance.

A modern front-end workflow would be as follows:

* Using {% link Bower http://bower.io/ true %} to easily manage the packages your project depends on.
* Using CSS and JS preprocessors for code modularity and simplicity, for example {% link Less http://lesscss.org/ true %} and {% link CoffeeScript http://coffeescript.org/ true %}.
* Using a task runner, {% link Gulp http://gulpjs.com/ true %} or {% link Grunt http://gruntjs.com/ true %}, to automate assets compilation and minification.

This was my personal setup a while ago before changing some parts recently.

As you probably know, {% link npm https://www.npmjs.com/ true %} is the larget ecosystem of open source libraries in the world. And alongside being a powerful package manager, npm can also be used as a simple script runner, which we can benefit from to avoid the need of any other task runners. Furthermore, many people mistake npm as being a package manager for back-end libraries only, or as being the "Node Package Manager", that's totally wrong! npm contains all open source libraries related to HTML, CSS, and JavaScript. You can read more about that in this {% link post http://blog.npmjs.org/post/101775448305/npm-and-front-end-packaging true %} from npm's blog.

With that being said, It's clear now that you don't really need all the tools I stated above to manage your front-end project, you can literally use npm to do everything and that's what we will do now! We'll create a simple website by managing everything using npm!

## The setup

The first step to start building our website is, of course, to create a folder where we will put everything, then we will use **npm init** to set it up:

{% codeblock lang:bash %}
$ mkdir Lab/website && cd Lab/website
$ npm init
{% endcodeblock %}

{% asset_img npm-init.png %}

**npm init** will take you trough some simple steps to create your **package.json** file, this is the file that will keep track of all your project's dependencies, and this is where you will also be writing the tasks you want to automate.

Let's now take care of setting up our project's structure:

{% asset_img structure.png %}

As you can see from the picture, we will have two main folders, **src** and **dist**. The first one will contain the plain source code of our website, while the second one will contain the production version with compiled and minified assets.

## HTML

The next step is to setup our **index.html** page:

{% codeblock lang:html website/src/index.html %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My website</title>
    <link rel="stylesheet" href="css/style.css" charset="utf-8">
  </head>
  <body>
    <p>Hello World!</p>
    <script src="js/bundle.js" charset="utf-8"></script>
  </body>
</html>
{% endcodeblock %}

As you might have noticed, I have put just one **link** and one **script** tags, because the idea is that all our CSS and JavaScript files will be compiled and concatenated into one single file. This will make our website renders faster because we will make less request to the server and we'll have less files to fetch!

Now is the time to minify our cool HTML page and save it to the dist folder! For this I'm going to install a package called {% link html-minifier https://www.npmjs.com/package/html-minifier true %} from npm:

{% codeblock lang:bash %}
$ npm install html-minifier --save-dev
{% endcodeblock %}

The reason I added **--save-dev** is to save the package as a development dependency in our **package.json** file, this also adds the package to the **./node_modules/.bin** folder so it can be executed using npm.

Now to minify our HTML page we should normally type the following line in the terminal:

{% codeblock lang:bash %}
$ html-minifier --collapse-whitespace ./src/index.html -o ./dist/index.html
{% endcodeblock %}

Tried it? Didn't work, right? The reason it didn't work is because we didn't install the package globally and your system doesn't know where to find **html-minifier**. We should instead add the line above to our **package.json** file as a script and run it using npm:

{% codeblock lang:javascript website/package.json %}
{
  "name": "website",
  "version": "1.0.0",
  "description": "My npm based website",
  "scripts": {
    "html": "html-minifier --collapse-whitespace ./src/index.html -o ./dist/index.html"
  },
  "author": "Youssef Kababe",
  "devDependencies": {
    "html-minifier": "^0.7.2"
  }
}
{% endcodeblock %}

You can give your script any name you like, I chose "html" just for simplicity. We can now execute our script like this:

{% codeblock lang:bash %}
$ npm run html
{% endcodeblock %}

{% asset_img npm-run-html.png %}

You will then find your minified web page inside the **dist** folder. Minification removes all the unnecessary stuff like comments and white spaces, thus producing files with smaller size, which gives our website's loading speed an important boost.

## CSS

I'm going to use {% link Stylus https://learnboost.github.io/stylus/ true %} as my CSS preprocessor, so we'll start by installing the {% link Stylus https://www.npmjs.com/package/stylus true %} package from npm:

{% codeblock lang:bash %}
$ npm install stylus --save-dev
{% endcodeblock %}

You should always make your style files modular, which means you need to have one main file that includes styles from other files in the directory. I usually create a folder called "includes" where I put all the files that I will be including.

{% asset_img css-includes.png %}

{% codeblock lang:stylus website/src/css/includes/paragraphs.styl %}
body
  p
    color #fff
{% endcodeblock %}

{% codeblock lang:stylus website/src/css/main.styl %}
@import "includes/paragraphs"

bkgdcolor = #2481A3

body
  background bkgdcolor
{% endcodeblock %}

The next thing we should do, as you might have guessed, is to add a task to compile and minify our CSS:

{% codeblock lang:javascript website/package.json %}
...
"scripts": {
  ...
  "css": "stylus --include ./node_modules --compress ./src/css/main.styl -o ./dist/css/style.css"
},
...
{% endcodeblock %}

The last thing to do to compile your CSS is to type "**npm run css**" in your terminal, then you should able to see your compiled and minified code in the **dist** directory:

{% asset_img npm-run-css.png %}

Notice that I added "**--include ./node_modules --include-css**" to our script, the reason for this is that you might want to include CSS files from other npm packages. For example, let's say we want to add {% link Bootstrap http://getbootstrap.com/ true %} to our project:

{% codeblock lang:bash %}
$ npm install bootstrap --save
{% endcodeblock %}

This time I used **--save** instead of **--save-dev**, that's because we're not going to use Bootstrap in a npm script. npm will now take care of fetching the package with all it's dependencies (except development dependencies) and place everything in the **./node_modules** folder.

The next step is to include Bootstrap's CSS file in our **main.styl** file:

{% codeblock lang:stylus website/src/css/main.styl %}
@import "bootstrap/dist/css/bootstrap.min.css"
...
{% endcodeblock %}

We're done! We have now successfuly added BootStrap's CSS to our project. When you compile your CSS next time, Stylus will look for "**bootstrap/dist/css/bootstrap.min.css**" inside **./node_modules** and include it. You see how easy it is? No more downloading, extracting, or copying!

## JavaScript

I'm going to use just plain JavaScript in my project, with the help of {% link Browserify http://browserify.org/ true %} and {% link UglifyJS http://lisperator.net/uglifyjs/ true %}.

Browserify is a tool that lets you include npm JavaScript modules in the browser using the {% link CommonJS http://www.commonjs.org/specs/modules/1.0/ true %} **require** style, so you can require modules and other JavaScript files like you would do when developping a {% link Node.js https://nodejs.org/en/ true %} application.

Then we will pipe Browserify's outpute through UglifyJS to have a minified JavaScript files.

Let's install Browserify and UglifyJS now:

{% codeblock lang:bash %}
$ npm install browserify uglify-js --save-dev
{% endcodeblock %}


We'll include Bootstrap's JavaScript from the package we installed earlier in the CSS section, and because Bootstrap depends on {% link jQuery https://jquery.com/ true %}, we also have to install it too:

{% codeblock lang:bash %}
$ npm install jquery --save
{% endcodeblock %}

Now we're ready to write our main JavaScript file.

{% codeblock lang:javascript website/src/js/main.js %}
var $ = require('jquery');
global.jQuery = global.$ = $;

var bootstrap = require('bootstrap');

alert("Hello World!");
{% endcodeblock %}

As you can see from line 2, I exposed jQuery as a global variable, if we didn't do this, we'll get a "**ReferenceError**" telling us that jQuery was not defined. That's because Browserify bundles your required libraries in a way that they can never conflict with each other, which makes libraries that depend on jQuery unable to find it. So you have to manually expose jQuery to the global (window) object.

You can do the same thing we did with CSS and create a folder called "**includes**" to keep your code modular instead of putting all the JavaScript in the main file.

Now to compile and minify our JavaScript file, we'll add the following script to the **package.json** file:

{% codeblock lang:javascript website/package.json %}
...
"scripts": {
  ...
  "js": "browserify ./src/js/main.js | uglifyjs > ./dist/js/bundle.js"
},
...
{% endcodeblock %}

## Fonts

Some npm packages like {% link Bootstrap https://www.npmjs.com/package/bootstrap true %} and {% link "Font Awesome" https://www.npmjs.com/package/font-awesome true %} come with font files bundled in them. For now, if you try to add a Bootstrap icon to your website, it will not render correctly because Bootstrap fonts are missing.

Unless you're not too lazy like me, you really don't want to be copying font files manually from the **./node_modules** folder, for each package, to put them inside your **dist** folder. As a workaround to this issue, I ended up making a little package called {% link Fontify https://www.npmjs.com/package/fontify true %} which will do that for you:

{% codeblock lang:bash %}
$ npm install fontify --save-dev
{% endcodeblock %}

Fontify requires a little configuration file called "**fontify.json**" on the root directory of your project, you will use this file to tell Fontify which packages you want to copy font files from, and where you want to put them:

{% codeblock lang:javascript website/fontify.json %}
{
  "modules": [
    "bootstrap"
  ],
  "dest": "dist"
}
{% endcodeblock %}

Finally we'll add a npm script for this:

{% codeblock lang:javascript website/package.json %}
...
"scripts": {
  ...
  "fonts": "fontify"
},
...
{% endcodeblock %}

Now type "**npm run fonts**" in your terminal and you'll find that all Bootstrap fonts were carefully copied to **./dist/fonts** folder:

{% asset_img fonts.png %}

## Automate the build process

Automation has been keeping developers from losing their minds and going crazy for ages! This is why we won't be executing our scripts manually one by one everytime. We will instead automate the building process step by step.

First thing to do is to make a script that will build our entire project at once:

{% codeblock lang:javascript website/package.json %}
...
"scripts": {
  ...
  "build": "npm run html & npm run css & npm run js & npm run fonts"
},
...
{% endcodeblock %}

We will continue further with the automation process by watching assets files and executing scripts automatically when any change happens. To accomplish this task, we will use the {% link Nodemon https://www.npmjs.com/package/nodemon true %} package from npm as it's the right tool for this job:

{% codeblock lang:bash %}
$ npm install nodemon --save-dev
{% endcodeblock %}

{% codeblock lang:javascript website/package.json %}
...
"scripts": {
  ...
  "watch-html": "nodemon --ext html --watch ./src/ --exec 'npm run html'",
  "watch-css": "nodemon --ext styl --watch ./src/css --exec 'npm run css'",
  "watch-js": "nodemon --ext js --watch ./src/js --exec 'npm run js'",
  "watch": "npm run watch-html & npm run watch-css & npm run watch-js"
},
...
{% endcodeblock %}

Now when you type "**npm run watch**", Nodemon will start watching all our assets and will execute the right command depending on what has been changed in your project directory:

{% asset_img npm-run-watch.png %}

## Live development server

It would be awesome if our web page could refresh itself automatically when we make some changes instead of manually refreshing it everytime! {% link live-server https://www.npmjs.com/package/live-server true %} is an awesome development server with live reload capability, so we're going to install it and use it for our project:

{% codeblock lang:bash %}
$ npm install live-server --save-dev
{% endcodeblock %}

Then we'll add a script that will watch our assets and lunch the development at the same time:

{% codeblock lang:javascript website/package.json %}
...
"scripts": {
  ...
  "server": "npm run watch & live-server --port=3000 ./dist"
},
...
{% endcodeblock %}

All you should do now is typing "**npm run server**" in your terminal, this will make Nodemon watch you source files for changes and recompile them automaticall, and launch a live development server on port **3000**, the server will be watching files in the **./dist** folder for changes, and will reload your page whenever it detects something.

## The end

Your final **package.json** file should look something like this:

{% codeblock lang:javascript website/package.json %}
{
  "name": "website",
  "version": "1.0.0",
  "description": "My npm based website",
  "scripts": {
    "html": "html-minifier --collapse-whitespace ./src/index.html -o ./dist/index.html",
    "css": "stylus --include ./node_modules --include-css --compress ./src/css/main.styl -o ./dist/css/style.css",
    "js": "browserify ./src/js/main.js | uglifyjs > ./dist/js/bundle.js",
    "fonts": "fontify",
    "build": "npm run html & npm run css & npm run js & npm run fonts",
    "watch-html": "nodemon -e html -w ./src/ -x 'npm run html'",
    "watch-css": "nodemon -e styl -w ./src/css -x 'npm run css'",
    "watch-js": "nodemon -e js -w ./src/js -x 'npm run js'",
    "watch": "npm run watch-html & npm run watch-css & npm run watch-js",
    "server": "npm run watch & live-server --port=3000 ./dist"
  },
  "author": "Youssef Kababe",
  "devDependencies": {
    "browserify": "^11.1.0",
    "fontify": "0.0.2",
    "html-minifier": "^0.7.2",
    "live-server": "^0.8.1",
    "stylus": "^0.52.4",
    "uglify-js": "^2.4.24"
  },
  "dependencies": {
    "bootstrap": "^3.3.5",
    "jquery": "^2.1.4"
  }
}
{% endcodeblock %}

Using stylus and plain JavaScript is just a matter of choice, you can replace them with anything you want and you'll always find tha packages you need on {% link npm https://www.npmjs.com true %}. You can also structure your project in any way that suits you the best!

Another thing you can do is to use what we did in this tutorial as a reusable skeleton for your future front-end projects instead of starting from scratch each time.
