---
layout: post
title:  "A Pragmatic Approach To Building Ruby\rails Apps Quickly With Quality Code"
date:   2016-06-09 11:47:06 -0300
categories: jekyll update
---

<h2 class="post-subheading">Good ruby gems to improve your code quality and performance</h2>

<article role="main" class="blog-post">
<p Excerpt>(This post appeared first <a href="http://www.codelitt.com/blog/pragmatic-approach-building-ruby-rails-apps-quickly-quality-code/">here</a>)</p>
<p>Startups often place priority with speed over nearly everything. They move at astronomical speed so they can hurry into the market for proper validation. Startups product code bases or features usually fall into two categories because of this:</p>
<p><strong>In production/maintain it or under active development</strong></p Out-of-excerp>
<p>At the early stages of a product, you’re very rarely taking care of tech debt. What works is running with as little maintanence (hopefully) as possible and the majority of efforts are focused on new parts of the product.</p>
<p>The problem with this, of course, is the quality of the project in general mostly when it comes to code quality and security.</p>
<p>The founder of <a href="www.linkedin.com">LinkedIn</a> <a href="https://www.linkedin.com/in/reidhoffman">Reid Hoffman</a> is often quoted as saying:</p>
<p><em>If you are not embarrassed by the first version of your product, you’ve launched too late.</em></p>
<p>(More about this philosophy <a href="http://www.businessinsider.com/the-iterate-fast-and-release-often-philosophy-of-entrepreneurship-2009-11">here</a>)</p>
<p>Sometimes, this quote is taken a little too literally. If you’re delivering on your promise then it’s okay to be embarrassed about the current state of your product, but you don’t necessarily have to be as embarrassed by your code quality. It is obvious that corners will be cut and tech debt will accrue for the sake of speed to market, but there are small practices that <em>can</em> ensure maintainability, elegance, and actually save time (our most expensive resource) and money so you can focus on new features.</p>
<p>Here at <a href="codelitt.com">Codelitt Incubator</a> we have a lot of projects built with Ruby/Rails somewhere in the stack. This sums up our reasoning <a href="https://www.quora.com/Why-do-so-many-startups-use-Ruby-on-Rails">here</a>. Whether its our R&amp;D work with large companies to our startup work, we’re constantly working on early stage products. Because of this, we’ve defined a set of gems that should be in every project in order to help us to maintain our best practices, eliminate headaches later on, and still move fast.</p>
<h3 id="code-quality">Code quality</h3>
<p>Just saying quality is rather abstract, so we’ve decided to focus in three main points:</p>
<ol>
<li>Readability</li>
<li>Organization</li>
<li>Maintainability</li>
</ol>
<p>Our code must be <strong>readable</strong> so our engineers don’t waste time trying to understand what it does.</p>
<p>Our code must be well <strong>organized</strong> in packages/modules/classes. We strive to follow the rules of the <a href="http://guides.rubyonrails.org/index.html">Rails Style Guide</a>.</p>
<p>Our code must be <strong>maintainable</strong>. Although we work with early stage products, we do care about the future of each one. We would bite ourselves in the ass because most products grow and evolve. You’ll finish feature 10 and circle back to improve the first feature you built.</p>
<h5 id="rubocop"><a href="https://github.com/bbatsov/rubocop">Rubocop</a></h5>
<p>RuboCop is a Ruby static code analyzer, it searches for code smell and bad practices as defined by the community.
Examples of smells are:</p>
<ol>
<li>Method too complex</li>
<li>Line too long</li>
<li>Wrong spacing between lines or variables</li>
</ol>
<p>The gem has a huge range of best practices and allows us to customize them, like in the example below:</p>
<div class="highlighter-rouge"><pre class="highlight"><code>Rails:
 Enabled: true

Metrics/LineLength:
 Max: 120

Style/Documentation:
 Enabled: false

Style/BlockDelimiters:
 Enabled: false
</code></pre>
</div>
<p>Besides showing the code smell, it offers a great auto fix system that when used with Rails will update the code changing something like:</p>
<div class="language-ruby highlighter-rouge"><pre class="highlight"><code><span class="k">def</span> <span class="nf">my_attribute</span>
 <span class="n">my_attribute</span>
<span class="k">end</span>
</code></pre>
</div>
<p>into</p>
<div class="language-ruby highlighter-rouge"><pre class="highlight"><code> <span class="kp">attr_accessor</span> <span class="ss">:my_attribute</span>
</code></pre>
</div>
<p>I started using it with huge, legacy project that was already in production. It’s auto fix took the code base from 1400 warnings to 214. It fixed more than 80% of the code smell. Most of the skipped problems were too complex that I had to correct, but it gives me an automated way to keep track of all of the issues I need to take care of. When ran from the beginning of a project, it is easy to quickly fix issues along the way.</p>
<p>Alongside with <a href="https://github.com/bbatsov/rubocop">Rubocop</a> we use <a href="https://github.com/nevir/rubocop-rspec">Rubocop Rspec</a> because it has a lot of RSpec specific best practices.</p>
<p>These act as a watchdog for us. Even engineers with years of experience make small mistakes like the wrong use of indentation, commas, variables declared but not used, etc… We have a policy of 0 warnings in our CI, so before the build rubocop runs and if a warning is raised the build fails. We always know what to expect when jumping into another teammates projects and a lot of the younger guys have learned a lot from it.</p>
<h5 id="rubycritic"><a href="https://github.com/whitesmith/rubycritic">Rubycritic</a></h5>
<p>Rubycritic is another static code analyzer, it wraps around the static analysis gems <a href="https://github.com/troessner/reek">Reek</a>, <a href="https://github.com/seattlerb/flay">Flay</a> and <a href="https://github.com/seattlerb/flog">Flog</a>. We mainly use this gem because it offers an overview of the duplicated code and complexity, as you can see in the report below:</p>
<p><img src="https://raw.githubusercontent.com/kaiomagalhaes/kaiomagalhaes.github.io/master/_posts/images/code_quality.png" alt="alt text"></p>
<p>We care a lot about the <a href="https://en.wikipedia.org/wiki/Don%27t_repeat_yourself">DRY</a> principle and the report that this gem offers is really helpful to make sure we’re not over complicating things or duplicating functionality throughout the code base.</p>
<h3 id="security">Security</h3>
<p>We work with projects from ML to robotics to static splash pages to money transfer systems, but regardless security is important in all of them.</p>
<p>Besides our <a href="https://github.com/codelittinc/incubator-resources/blob/master/best_practices/servers.md">server best practices</a> and data storage best practices, another possibly entry point is the harm that may be done to our applications by attacks with: DDOS, SQL Injection and so on. While I in NO WAY recommend these be your only security review (just as the above gems should not be soley responsible for your code quality) we use the following gems to sniff out the obvious stuff:</p>
<h6 id="brakeman"><a href="https://github.com/presidentbeef/brakeman">Brakeman</a></h6>
<p>Brakeman is a gem that checks Ruby on Rails applications for security vulnerabilities, it raises warnings for each one that is found.</p>
<p>The policy for us is 0 security warnings. As it is the first step in our build process, if it finds any glaring security gap then it fails and won’t continue to build.</p>
<h6 id="dawnscanner"><a href="https://github.com/thesp0nge/dawnscanner">Dawnscanner</a></h6>
<p>Dawnscanner is a source code scanner designed to review ruby code for security issues. We use it alongside <a href="https://github.com/presidentbeef/brakeman">Brakeman</a> to ensure a redundancy in the automated portion of our security review. It runs after <a href="https://github.com/presidentbeef/brakeman">Brakeman</a> and fails if it finds any gap as well.</p>
<h6 id="bundler-audit"><a href="https://github.com/rubysec/bundler-audit">Bundler-audit</a></h6>
<p>Bundler Audit checks the Gemfile.lock and searches for security issues that have been reported in each gem used in the project. If it finds any problem in the version of a gem, it searches for a patch level that is secure and asks us to update it. If a secure one doesn’t exist, well, it is time to search for another gem completely.</p>
<p>While we focus on the general security of our applications it is common to forget that the product is a combination of gems, libraries, and code that we write, so if any gem used in the project has a security gap then it is a trojan horse that can sink our best code.</p>
<p><a href="https://github.com/rubysec/bundler-audit">Bundler-audit</a> is the third step before the build. When it finds a compromised gem, we search for a safe version, in the case of not finding one, we just throw it away and search for another way to solve the problem.</p>
<h4 id="test-coverage">Test coverage</h4>
<p>Test coverage is important because with this data we can ensure that our application is being minimally tested. As code lines covered doesn’t guarantee that the critical points of the application are being tested (which should be our main focus), we can’t use it as basis to make sure that our applications are entirely tested and set, but these gems provide a fairly decent overview that usually can identify weak points. Tests save us a huge amount of QA if only because we know something that was working will keep working after some change in the code.</p>
<h5 id="simplecov"><a href="https://github.com/colszowka/simplecov">Simplecov</a></h5>
<p>SimpleCov is a code coverage analysis tool for Ruby. Besides offering good test reports, it allows us to configure our tests to fail if a specified coverage percentage is not met.</p>
<p>We define that the minimum code coverage should be at least 90%.</p>
<h3 id="summary">Summary</h3>
<p>These are the principles and gems that we use in our Ruby/Rails projects here at <a href="codelitt.com">Codelitt</a>. It will be updated every time that we find new gems or good practices. Again, these do not replace proper code reviews, pen testing, refactoring, etc, (we’ll cover that in a future article) but they do provide an automated way for you to move fast and have a certain level of confidence in your code. If you know a better solution or have some recommendation please let me know in the comments or by email. I would really appreciate it: kaio@codelitt.com.</p>
<p>If you want to know more about our best practices, take a look in our <a href="https://github.com/codelittinc/incubator-resources">repository</a>. We open source all of our policies and best practices as well as continue to add to them there.</p>
</article>