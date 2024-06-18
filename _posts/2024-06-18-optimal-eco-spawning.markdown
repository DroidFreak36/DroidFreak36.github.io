---
layout: single
title:  "Optimal Eco Spawning"
date:   2024-06-18 0:16:34 -0500
categories: jekyll update
---


It seems that understanding the theory behind spawning systems is difficult for a lot of players, so this post is intended to help with that. I believe that with this theory, it is possible to get spawning decisions that are quite optimal. The focus here is solely on eco spawning. Other types of spawning can be fit in with whatever priority seems appropriate and the eco spawning can scale down appropriately, still using the theory outlined in this post to make the most of the available spawn time.

<h2>{{ "Spawning Targets" }}</h2>

The first thing that we need to figure out is what we want our total population of creeps to look like. This is important to do because without a coherent idea of what we want the total population to be, we will probably end up with an inefficient ratio of roles. We can keep things running smoothly by achieving a good balance between different categories of creeps. 

<h4>{{ "Categories" }}</h4>

Speaking of which, what are the categories we need to achieve balance between? I think they can (and should) be neatly divided into these three categories:

1) Energy production<br>
2) Transportation<br>
3) Energy usage

We want the energy production and usage to be balanced so that we don't have energy going to waste or workers sitting idle, and we want to have enough transportation to get the energy where it needs to go without haulers sitting idle. So these three categories are ultimately what we want to balance for spawning, to make sure that our economy is achieving maximum productivity on whatever type of work we are doing. That type of work may vary, but I think it it always beneficial to balance worker spawning as a whole rather than individual categories, since our energy spending as a whole is what we need to balance against our production and transportation.

<h4>{{ "Units" }}</h4>

For energy production, it's quantified in terms of how many sources we're trying to mine, and whether or not the rooms are reserved. The only decisions that effect this category are decisions about when we should add or remove sources, or change the reservation status of a source, and those decisions have a ripple effect across the whole economy, so this category determines the magnitude of our economy as a whole, asuming that the other categories are scaled appropriately. This is the category that we directly adjust when we have macro decisions to make, such as reducing total spawn usage or total CPU usage by dropping a remote, or using addition spawn time and CPU to get a larger economy.

For transportation, we care about the total carry capacity of all of our haulers (assuming that they all move at full speed in most cases, which should be true if you're using appropriate bodies for your road status). This is equivalent to the number of carry parts across all of our haulers (with a multiplier for boosts, if you are using any). So we represent the target for this category as the target number of carry parts.

For energy usage, we can mostly use a similar scheme as what we use for haulers, counting work parts instead of carry parts, but there is one notable exception - when work parts are used for building, they use 5 energy per tick instead of the 1 energy per tick used for other tasks. So we need to count them as 5 times as much. And that means that rather than directly being target work parts, we quantify workers in terms of target energy spending per tick.

<h3>{{ "Regulation (Haulers and Workers)" }}</h3>

There are two main approaches to regulating how much we should spawn of each category -

1) Calculating how much we need of each category based on expectations
2) Observing and reacting to the real game conditions

Both of those approaches have advantages, so I think the optimal approach is to do both! That is, I think we should keep a target value in memory for each category and adjust it in two different ways -

1) Whenever we make a change to our energy income, we apply an adjustment to our target values for the other categories based on our calculated expectations.
2) Whenever we observe a surplus or shortage of a category, we apply an adjustment to the target values based on that observation.

With that combined approach, we end up with a target that is close to optimal after every adjustment to our income (from our calculated expectation), but we also have the reactive system to hone in on exactly the right value for the real game conditions.

<h3>{{ "Regulation (Miners and Reservers)" }}</h3>

This category is different, because it affects the entire scale of our economy. We increase it when we think we can handle more, and decrease it when we think we can't handle as much. This can be constrained by either spawn time or CPU (or we could drop remotes due to danger). Another thing of note is that changing the reservation status of a remote room is a change to energy income, and should be treated accordingly: The targets for haulers and workers should react, and if the change puts us beyond our constraints, we should scale back to stay inside our constraints.

<h2>{{ "Spawning Priorities" }}</h2>

