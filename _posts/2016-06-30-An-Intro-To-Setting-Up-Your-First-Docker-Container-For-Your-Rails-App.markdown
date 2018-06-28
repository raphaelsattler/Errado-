---
layout: post
title:  "An Intro To Setting Up Your First Docker Container For Your Rails App"
date:   2016-06-30 11:47:06 -0300
categories: jekyll update
---

<h2 class="post-subheading">A tutorial about how to setup your first docker container for your rails application</h2>

<article role="main" class="blog-post">
<p>Since the <a href="https://upload.wikimedia.org/wikipedia/commons/c/c2/Lambda-Cold_Dark_Matter,_Accelerated_Expansion_of_the_Universe,_Big_Bang-Inflation.jpg">beginning of the time</a> (Disclaimer: possibly not an accurate timeline), we have been suffering from the <em>‘But it’s working in my machine’</em> curse. It happens because we are probably not going to deploy the application in our local machine. Our local machine usually happens to be different than the production one. It can be different in lots of ways e.g: operational system, environment variables, installed dependencies and the list continues.</p>
<p>To minimize this problem here at <a href="http://codelitt.com">Codelitt</a> we develop and deploy our applications inside docker containers. It started as a bit of a grassroots movement among employees, but the movement has really taken hold. From simple Jekyll pages, to frontend clients, to blue-green deployment for services, we’ve Dockerized <strong>everything</strong>.</p>
<p><img src="https://i.imgur.com/FiNj8.jpg" alt="Dockerize all the things"></p>
<p>For those not terribly family with Docker, you can read more on their <a href="https://www.docker.com/what-docker">site</a>, but from the horses mouth:</p>
<div class="highlighter-rouge"><pre class="highlight"><code>Docker containers wrap up a piece of software in a complete filesystem that
contains everything it needs to run: code, runtime, system tools,
system libraries – anything you can install on a server. This guarantees that it
will always run the same, regardless of the environment it is running in.
</code></pre>
</div>
<p>There are many benefits of Docker, but what has always stood out to us is the fact <strong>“it will always run the same on any machine”</strong>. It is like a salve in our old wounds. That ghost of “It works on my machine” fades away and we can focus on things that matter.</p>
<p>We wanted to explain how we develop and setup our applications to production with Docker. We have a set of best practices and have automated our deployment and implemented <a href="http://martinfowler.com/bliki/BlueGreenDeployment.html">blue-green deploys</a> with it, but in this first article we’re going to cover getting Dockerfiles setup for a project. Dockerfiles are just a set of instructions which build the system in a predictable and repeatable way across engineers’ machines (and production machines).</p>
<p>In this tutorial we are going use a Ruby on Rails application as example.</p>
<p>First you need to <a href="https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04">install docker</a>. It’s straight forward enough so we’ll not cover that for now.</p>
<p>All of our applications have at least one Docker container, which has all the necessary information to build it – most have a few. Docker really becomes powerful when you start to use a container per process. It separates concerns, allows for great scalability, agility, resilience, and speed. Containers can be easily coordinated with Docker Compose, but we’ll cover that in another article. For now let’s setup two Dockerfiles. One for development and one for prod</p>
<h4 id="development">Development</h4>
<div class="highlighter-rouge"><pre class="highlight"><code># This is our DEVELOPMENT dockerfile.
#
# This uses Codelitt's Ruby 2.2 image found at:
# https://github.com/codelittinc/dockerfiles/blob/master/ruby/Dockerfile
FROM codelittinc/ruby:2.2
MAINTAINER Codelitt, Inc.

# Mount any shared volumes from host to container @ /share
VOLUME ["/share"]

# Install dependencies and rails-api
RUN apt-get update \
    &amp;&amp; rm -rf /var/lib/apt/lists/* \
    &amp;&amp; gem install --no-rdoc --no-ri \
    rails-api

WORKDIR /share

CMD ["/bin/bash", "-l"]
</code></pre>
</div>
<p>It is a Dockerfile for a Ruby application. Add it to you application’s directory and build it.</p>
<h3 id="building">Building</h3>
<p>As it is a Ruby on Rails application you are probably going to need a database. Now normally at this point in a Rails app, you’re going to have to install a database on your machine. The beauty of Docker is you no longer have to go through that pain. Just fire up a database container straight from the developers themselves!</p>
<p>Let’s run a postgres database on docker</p>
<p><code class="highlighter-rouge">docker run --name myapp_db -e POSTGRES_PASSWORD=postgres -d postgres</code></p>
<p>This command will download the official postgres image and run a container based on it, as well as open port 5432 for connections.</p>
<p>Now let’s run the application.</p>
<p>First you need to build the application image. Just like the postgres image, this will be the image that your container is based on.</p>
<p><code class="highlighter-rouge">docker build -t myapp .</code></p>
<p>And now you run it</p>
<div class="highlighter-rouge"><pre class="highlight"><code>docker run -d \
-ti \
--name myapp \
-e MYAPP_DATABASE_PASSWORD=postgres \
-e MYAPP_DATABASE_USER=postgres \
-v $(pwd):/share \
-p 3000:3000 \
--link myapp_db:db myapp /bin/bash -l
</code></pre>
</div>
<p>Now you have the container running and connected to your database via the –link flag. the -p flag exposes port 3000 from the container to port 3000 on your host machine. Your environment variables are set via the -e flag. -ti gives you an interactive terminal waiting for you from within your container. -v is going to connect your current directory to the /share directory inside the container.</p>
<p>Oh but how do I access it? It is easy, just run:</p>
<p><code class="highlighter-rouge">docker exec -it myapp bash</code></p>
<p>Now you’ll be in the /share folder of your container which is shared with your application’s directory on your host machine. You have access to your application from within the container! You just need to setup it e.g: bundle install, rake db:create and so on.</p>
<p>To be able to create and work with the database you need to setup your config/database.yml to be like</p>
<div class="highlighter-rouge"><pre class="highlight"><code>default: &amp;default
  adapter: postgresql
  encoding: unicode
  host: db
  pool: 5
  user: &lt;%= ENV['MYAPP__DATABASE_USER'] %&gt;
  password: &lt;%= ENV['MYAPP__DATABASE_PASSWORD'] %&gt;

development:
  &lt;&lt;: *default
  database: myapp_development

test:
  &lt;&lt;: *default
  database: myapp_test

production:
  &lt;&lt;: *default
  database: &lt;%= ENV['MYAPP__DATABASE_NAME'] %&gt;
</code></pre>
</div>
<p>Note that we have setup the password in the command:</p>
<div class="highlighter-rouge"><pre class="highlight"><code>-e MYAPP_DATABASE_PASSWORD=postgres \
-e MYAPP_DATABASE_USER=postgres \
</code></pre>
</div>
<p>To create the database run: <code class="highlighter-rouge">rake db:create &amp;&amp; rake db:migrate</code> inside your machine.</p>
<p>To run the application: rails s -p 3000 -b `hostname -i`</p>
<p>Now you can access your application in the address: <code class="highlighter-rouge">localhost:3000</code></p>
<p>It is simple as that. Now when you format your computer, work with a teammate, or switch from project to project, you only need to fire up a container. You don’t need to install all the dependencies of your 20 projects to your host machine. All of the environment is contained in the container.</p>
<h4 id="production">Production</h4>
<p>When you’re ready to move to production, you’ll still want to secure your host server. Containers are only as secure as their host. We have a good tutorial about our <a href="http://www.codelitt.com/blog/my-first-10-minutes-on-a-server-primer-for-securing-ubuntu/">first 10 minutes on a server</a> which should take care of most basic server security concerns.</p>
<p>The production Dockerfile for the same application would be:</p>
<div class="highlighter-rouge"><pre class="highlight"><code># This is our Production dockerfile.
#
# This uses Codelitt's Ruby 2.2 image found at:
# https://github.com/codelittinc/dockerfiles/blob/master/ruby/Dockerfile
FROM codelittinc/ruby:2.2
MAINTAINER Codelitt, Inc.

# Mount any shared volumes from host to container @ /share
VOLUME ["/share"]

# Install dependencies and rails-api
RUN apt-get update \
    &amp;&amp; apt-get install -y nodejs \
    &amp;&amp; rm -rf /var/lib/apt/lists/* \
    &amp;&amp; gem install --no-rdoc --no-ri rails-api \
    &amp;&amp; gem install --no-rdoc --no-ri puma

WORKDIR /share
ADD Gemfile /share/Gemfile
ADD Gemfile.lock /share/Gemfile.lock
RUN bundle install
ADD ./ /share

CMD cp config/database.yml.example config/database.yml
ENV RAILS_ENV production
ENV SECRET_KEY_BASE MY_UNSAFE_SECRET
CMD rails s Puma -b 0.0.0.0 -e production -p 4000
</code></pre>
</div>
<p>As it is a Rails application you’ll need to change the <strong>SECRET_KEY_BASE</strong> to another hash (unless you want us to know your application secret key)</p>
<p>If you are using UFW in your server <a href="http://www.codelitt.com/blog/my-first-10-minutes-on-a-server-primer-for-securing-ubuntu/">(like we recommend)</a> you should take a look at this <a href="https://docs.docker.com/engine/installation/linux/ubuntulinux/#enable-ufw-forwarding">Docker documentation</a> as well.</p>
<p>Build the container and there is your application running in production.</p>
<h4 id="summary">Summary</h4>
<p>Now that you know the basics of Docker you are able to develop in a near identical environment as your production environment. We’ll be following up on this article with follow ups on Docker compose to coordinate microservices, Docker blue-green deployment, how we do one command deploys, and how to secure Docker containers. In the mean time, take a look at our aforementioned server security article or <a href="www.codelitt.com/blog/pragmatic-approach-building-ruby-rails-apps-quickly-quality-code/">our pragmatic approach to building quality and secure Rails apps</a>. We explain how to improve security, code smell, and code quality for ruby/rails applications.</p>
<p>We’re making a big push to externalize our processes and best practices, if you’re interested in learning more take a look at our <a href="https://github.com/codelittinc/incubator-resources">repository</a>. We open source all of our policies and best practices as well as continue to add to them there.</p>
</article>