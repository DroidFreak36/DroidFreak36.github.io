---
layout: single
title:  "Optimal Eco Spawning"
date:   2024-06-18 0:16:34 -0500
categories: jekyll update
---


It seems that understanding the theory behind spawning systems is difficult for a lot of players, so this post is intended to help with that. I believe that with this theory, it is possible to get spawning decisions that are quite optimal. The focus here is solely on eco spawning. Other types of spawning can be fit in with whatever priority seems appropriate and the eco spawning can scale down appropriately, still using the theory outlined in this post to make the most of the available spawn time.

<h2>{{ "Spawning Targets" }}</h2>

The first thing that we need to figure out is what we want our total population of creeps to look like. This is important to do because without a coherent idea of what we want the total population to be, we will probably end up with an inefficient ratio of roles. We can keep things running smoothly by achieving a good balance between different categories of creeps. 

<h3>{{ "Categories" }}</h3>

Speaking of which, what are the categories we need to achieve balance between? I think they can (and should) be neatly divided into these three categories:

1) Energy production
2) Transportation
3) Energy usage

We want the energy production and usage to be balanced so that we don't have energy going to waste or workers sitting idle, and we want to have enough transportation to get the energy where it needs to go without haulers sitting idle. So these three categories are ultimately what we want to balance for spawning, to make sure that our economy is achieving maximum productivity on whatever type of work we are doing. That type of work may vary, but I think it it always beneficial to balance worker spawning as a whole rather than individual categories, since our energy spending as a whole is what we need to balance against our production and transportation.

<h3>{{ "Units" }}</h3>

For energy production, it's quantified in terms of how many sources we're trying to mine, and whether or not the rooms are reserved. The only decisions that effect this category are decisions about when we should add or remove sources, or change the reservation status of a source, and those decisions have a ripple effect across the whole economy, so this category determines the magnitude of our economy as a whole, asuming that the other categories are scaled appropriately. This is the category that we directly adjust when we have macro decisions to make, such as reducing total spawn usage or total CPU usage by dropping a remote, or using addition spawn time and CPU to get a larger economy.

For transportation, we care about the total carry capacity of all of our haulers (assuming that they all move at full speed in most cases, which should be true if you're using appropriate bodies for your road status). This is equivalent to the number of carry parts across all of our haulers (with a multiplier for boosts, if you are using any). So we represent the target for this category as the target number of carry parts.

For energy usage, we can mostly use a similar scheme as what we use for haulers, counting work parts instead of carry parts, but there is one notable exception - when work parts are used for building, they use 5 energy per tick instead of the 1 energy per tick used for other tasks. So we need to count them as 5 times as much. And that means that rather than directly being target work parts, we quantify workers in terms of target energy spending per tick.





You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
