---
layout: post
title:  "The Art Of Building Products"
date:   2016-11-28 11:47:06 -0300
categories: jekyll update
---

<h2 class="post-subheading">A base path for anyone willing to spend more time on selling the product instead of in building it</h2>

<article role="main" class="blog-post">
<p>In my <a href="http://kaiomagalhaes.com/2016-09-21-The-Art-Of-Defining-Products/">last article</a>, we talked about The art of defining products, which is your first step in the journey of having your product in the market (AKA fulfilling your dream). After you have your MVP defined you need to build it, otherwise it’s going to be just another idea sent to the 4th dimension (alongside your favorite pens and car keys).</p>
<p>Our team has collectively built hundreds of products. This is how we do it here at <a href="codelitt.com">Codelitt</a>.</p>
<p>First off, you need to keep in mind a simple but important concept: <a href="https://img.buzzfeed.com/buzzfeed-static/static/2014-07/17/16/enhanced/webdr04/anigif_enhanced-11135-1405627535-2.gif">products evolve</a>. No, I’m not talking about the changes made between the first and the second version released into the market; I’m talking about what you define before you start building it and what you want to be released.
Let’s use a house as an example:</p>
<p><img src="https://raw.githubusercontent.com/kaiomagalhaes/blog/master/en/the_art_of_defining_products/image00.jpg" alt="alt"></p>
<p>To build it you first need to create a plan including an elevation plan, floor plan, and water and energy plans. As houses and rooms are palpable and common, it isn’t hard to define exactly what you want. We often hear:</p>
<p>“I went to Claire’s house and I want one of those kitchen tables, but I want to put it into my dining room”</p>
<p>Simple isn’t it? Zero tests needed. If a table works for a kitchen, then it is probably going to work for a dining room. But what if you want to create something that you have never seen before and want to put things you’ve seen into it?</p>
<p>“I accessed the CNN site and I want one of those pretty news boxes on my site where I sell fresh fruits”</p>
<p>Wait a minute… add a news box into a place where you sell fruits?</p>
<p>It may work, but there are some things that need to be checked first:</p>
<ol>
<li>Is it necessary or just pretty?</li>
<li>Does it fit the design and information architecture?</li>
<li>Does it add value to the site?</li>
<li>Does it make sense to the user at all?</li>
</ol>
<p>It may sound nitpicky, but research has shown that the user takes less than a second to gather a first impression of your website<a href="http://www.anaandjelic.typepad.com/files/attention-web-designers-2.pdf">¹</a>, and that is just from the aesthetics. You need them to enjoy the aesthetics AND the site needs to be focused on accomplishing their goal. Naive product owners think they can make their products “pretty” and users will automatically love it, ignoring the fact that the experience is awful and it’s almost impossible to use.</p>
<p>In order to have all the product requirements in one place, first we gather the primary information using <a href="trello.com">Trello</a>:</p>
<p><img src="https://raw.githubusercontent.com/kaiomagalhaes/blog/master/en/the_art_of_defining_products/image04.png" alt="alt"></p>
<p>These are called epics. They are basically very broad and short descriptions of an individual feature. It is a high-level look at all of the features we are going to need for the final product. Epics make it easier to interview the product lead to get more details like: “Which product fields do you want to save?”; “How should the user edit page look?”; “What fields should the search function look at?” and so on.</p>
<p>As soon as we have this done, we validate it with the client and begin to write the user stories, which is where we get specific steps for each action. Many user stories comprise a single epic. This phase is crucial. User stories allow us to tell a narrative and this often catches glaring issues that we haven’t thought of if we just listed out high-level features (aka epics). Most importantly, it allows us to look at the product from a user’s perspective. This helps us identify issues they may run into or edge cases that were not obvious at first. We can then review the user stories and narrative with the product lead and client to make sure we have properly understood the vision.As an R&amp;D and innovation company, we’re experts in product and software, but the client brings their own expertise. They know what is needed to go to market.</p>
<p>Now that we have all the requirements validated, we add what is necessary to make it happen: Libraries that will be useful, data structures, API interfaces, deployment strategy, server configuration and so on. After we have all the tasks organized we split them into iterations with a predetermined time, usually weekly iterations - or sprints.</p>
<p>We show this in Trello which is a Kanban workflow software:</p>
<p><img src="https://raw.githubusercontent.com/kaiomagalhaes/blog/master/en/the_art_of_defining_products/image03.png" alt="alt"></p>
<p>However, estimating time is always hard. It takes a certain amount of science and a certain amount of art:</p>
<p><img src="https://raw.githubusercontent.com/kaiomagalhaes/blog/master/en/the_art_of_defining_products/image01.png" alt="alt"></p>
<p><a href="http://xkcd.com/">XKCD</a> has the idea.</p>
<p>Every developer has their own method of estimating development time.. There are different methods we’ve tried with varying degrees of success, however, that is for a future post.</p>
<p>Estimating can change MVP features because a single feature may take a huge portion of the resource, so it needs to be done carefully.</p>
<p><img src="https://raw.githubusercontent.com/kaiomagalhaes/blog/master/en/the_art_of_defining_products/image02.png" alt="alt"></p>
<p><a href="http://xkcd.com/">XKCD</a> has the idea (again).</p>
<p>Now that we have everything organized, we need to start on the implementation.</p>
<p>As I mentioned before, our sprints last 1 week. In this time, we aim to build all the features contained in the sprint, but sometimes it doesn’t happen. Why? Well, the answer is simple: We can’t predict the future. No software engineer can. The only thing we can predict is that it will be unpredictable. The implementation of a feature may take longer than expected, something from design/content/PM/etc. may be blocking progress. Once we had to implement a payment system using a gateway and in the middle of the implementation we discovered that they changed how their API worked, but didn’t update some dependencies. The result: We needed to stop in the middle of a half built feature and restart the integration with another payment system.</p>
<p>Working in the field of innovation often means steering through uncharted waters. Setbacks can be expected. Plans are meant to be broken, and rarely has a project turned out how it was originally planned and “specced”. Changes have historically come from all directions, whether they are internal or external. The key point for us is to adjust ourselves as quickly as possible to these circumstances, and be transparent about the process.</p>
<p>However, this does not mean that we should never add time to our estimates for potential setbacks. When we do encounter an unexpected issue, we immediately communicate it to the team and try to identify how it sets back the timeline. Adjusting timelines does not mean we don’t meet our deadlines; adjusting timelines means that we constantly maintain realistic expectations.</p>
<p>Another source of change will most likely be the customer themselves. Customers, especially involved ones, want to see their vision of the product rolled out. That means that they will often request changes or new features that were not included in the original scope. It is very easy to say, “Yes, we can do that,” especially if “that” is something minor. Small things can easily add up.</p>
<p>By no means does this mean that we say “yes” to all customer requests. Part of the value we bring to this field is our experience and expertise. If we do push back on a request, it is because we have researched it, found examples of it, and can professionally advise against it for objective reasons. This is particularly important with customers that tend to request changes because of their personal preference.</p>
<p>Good communication with the client is what make this process work. Every week we have a call where we explain:</p>
<ol>
<li>What is done and what isn’t.</li>
<li>The problems we faced.</li>
<li>What is expected to be done next week.</li>
<li>The new timeline if we had any roadblocks.</li>
</ol>
<p>This way we don’t need to add any “cushion” in our timeline.</p>
<p>This weekly system is also important to validate some visual features, flows, and so on. Remember what I said about changes in the product while it is being built? This is often where it comes into play. Once you see your idea taking shape you may find some better way to do something or discover that what sounded brilliant at first is not so spectacular after all. A huge benefit to short sprints and validating along the way is that product leads, clients, engineers are all satisfied at the end of the product because of the constant validation and input.</p>
<p>This is how we handle the product development here at codelitt. Sure, there are a ton of other things to talk about like the QA process, validation with the client, and sending the product to production, which are all things we are going to cover in future posts.</p>
</article>