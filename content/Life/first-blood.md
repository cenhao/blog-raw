Title: First Blood
Tags: blog, web
Slug: first-blood
Date: 2014-12-25 16:00

##**Finally have my blog set up!**

After working on the template thing for a week, I have the personal tech blog I *dreamt* for years. This is my first blog entry, and I'm gonna name it "First Blood"!!! (well, in fact I don't play Dota or anything alike;)

I don't mean that I've never written or had any blogs before, in fact, I've tried many different (forms of) blog. QZone at the very beginning(when I was still in my junior high), then 163, and CSDN & cnblogs later on when I started learning programming. I even set up my blog by using Wordpress on a VPS after that, several times.

But I never have the feeling that those blogs belong to me, I don't feel the tie between me and the blog. Those blogs are good, but that's all. As a coder, I always want to have more "**control**" over my personal blog, not only you can design all the details -- layout, syntax highlight, to name a few, but also I need to be able to do this through programming and a little help from some delicate open source tools. The solution has to be programmable, concise, portable and ultimately graceful.

And besides all of this, I want to host the blog myself. If one day the services are not longer provided, how can I rebuild my entire blog with minimum cost, having everything looks identical to the original one?

**None of the options I tried before meets my needs.**

Inspired by one of my friends, [keith yue](http://keithyue.github.io/pages/about-me.html), a talented PhD student in HKUST, I started looking into the pages services provided by Github.

*(I still want to make fun of the acronym, PhD is sort for Permanent Head Damage, haha)*

Basically Github Pages is a static site host service. You create a repo named *your_name/your_proj_name*.github.io, and commit static files to it, then you can access the site *your_name/your_proj_name*.github.io like all you files under the master branch are deployed on the root directory of your web server.

This is promising! Though I don't actually host my blog, I virtually do. Nowadays every coder has his own source hosting account, and Github is the most prevailing service provider beyond doubt. I own my repo, I get a hostname decent enough, and using static site generator is also a popular way to compose blog entries.

##**Building Blocks of this Blog**

###1. [Github Pages Service](https://pages.github.com/)
As mentioned above, to host all the static files, html, css, js, img, etc.

###2. [Pelican](http://blog.getpelican.com/)
Pelican is a static site generator written in Python, with Jinja2 as its template engine, supporting markdown syntax to compose blog entries.

There're lots of static site generator out there, the official recommended one from Github is Jekyll, but it's written in Ruby. I know nothing about this language, Python is a better option.

Pelican also supports some other marking language, but markdown is simple and powerful enough for blog. `reStructedText` is way more complex.

###3. [HTML5UP](http://html5up.net/)
HTML5 UP is where I found the template I wanted. The amazing part of its templates is the website scales/restructs automatically according to the width of the device, i.e., the blog has different layouts under different devices.

The downside of the templates provided there is that they are not implemented using frameworks like bootstrap. The author uses his own css & js. It makes the website more lightweight, but also makes the template less valuable to learn and harder to maintain.

Maybe later I'll rewrite the whole template using bootstrap, but that's another story.

###4. [jade](http://jade-lang.com/) & [pyjade](https://github.com/SyrusAkbary/pyjade)
Jade is the template engine for node.js, the jade-lang itself is a very elegant template language. Let's take a quick look on the difference between jade & jinja2.

This is jinja2 template:

	:::html
	<!DOCTYPE html>
	<!-- This is Jinja2 -->
	<html lang="en">
	  <head>
	    <title>{{ title }}</title>
	  </head>
	  <body>
	    <h1>
	      <a href="#">Example</a>
	    </h1>
	    <div id="container" class="col">
	      <p>More example</p>
	      <p>
	        there are loooooooots of text.
	        Veeeeery looooooong text.
	        Just to demonstrate.
	      </p>
	      <ul>
	      {% for item in list %}
	        <li>{{ item }}</li>
	      {% endfor %}
	    </div>
	  </body>
	</html>

This is jade template of the same content:

	:::jade
	doctype html
	  head
	    title= title
	  body
	    h1
	      a(href="#") Example
	    div.col#container
	      p More example
	      p.
	        there are loooooooots of text.
	        Veeeeery looooooong text.
	        Just to demonstrate.
	      ul
	        for (item in list)
	          li= item

Jinja2 is basically still a html document, while jade is more of a new notation system which uses indentation to struct the page.

If you're working with a front-end designer who passes you the finished pages to just add some JS to it so that it's dynamic, of course you would like jinja2 because you can slightly modify the pages a little bit and they become templates. This minimizes you work.

**But you're a full-stack developer who design your own pages, you'll find jade is way less painful to use and easier to understand.**

Pyjade converts jade templates to jinja2 templates. However it's not very mature and kinda lame. It does not support many basic jade syntax and sometimes I have to do some hack to the pyjade template to make the conversion work.

I wrote jade according to the template I found in HTML5 UP, and converted them into jinja2 templates by pyjade so as to generate my blog.

I'm planning to write a jade engine for Python myself, with full support to jade syntax and one can assembly it into his code, not simply being a converter. And I'm gonna name it **jade4py**.

###5. [DISQUS](https://disqus.com/)
DISQUS is comment platform and you can easily integrate it with the blog by add a few lines of html. It's pretty nice and you can see the effect at the bottom of this page.

However I encountered a problem that I can't change the color scheme to light background. I though it was my css that caused the problem, but after a meticulous investigation I found nothing. I gave up and set up another DISQUS for it, and everything turned out to be fine.

###6. Other Stuff
[Font Awesome](http://fortawesome.github.io/Font-Awesome/) and [Google Fonts API](https://www.google.com/fonts) is also used in this blog.

And I mainly use a free font named Nexa for display. the `g` letter looks fantastic in this font.

##Last Words
This is the first time I wrote an English blog and I'm sure there'll be lots of mistakes, and I'm also sure this would not be the last one. Everything is hard at the beginning and I have to undergo this process to escalate my English.

And I also learned a ton about front-end development during building the template, selecting the font, designing everything.

**I'm content with every aspect, merry christmas everyone!**
