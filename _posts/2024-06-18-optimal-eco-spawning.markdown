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

Next, what are the units that we use to quantify each category? That varies per category.

For energy production, it's quantified in terms of how many sources we're trying to mine, and whether or not the rooms are reserved. The only decisions that effect this category are decisions about when we should add or remove sources, or change the reservation status of a source, and those decisions have a ripple effect across the whole economy, so this category determines the magnitude of our economy as a whole, asuming that the other categories are scaled appropriately. This is the category that we directly adjust when we have macro decisions to make, such as reducing total spawn usage or total CPU usage by dropping a remote, or using addition spawn time and CPU to get a larger economy.

For transportation, we care about the total carry capacity of all of our haulers (assuming that they all move at full speed in most cases, which should be true if you're using appropriate bodies for your road status). This is equivalent to the number of carry parts across all of our haulers (with a multiplier for boosts, if you are using any). So we represent the target for this category as the target number of carry parts.

For energy usage, we can mostly use a similar scheme as what we use for haulers, counting work parts instead of carry parts, but there is one notable exception - when work parts are used for building, they use 5 energy per tick instead of the 1 energy per tick used for other tasks. So we need to count them as 5 times as much. And that means that rather than directly being target work parts, we quantify workers in terms of target energy spending per tick.

<h4>{{ "Regulation (Haulers and Workers)" }}</h4>

There are two main approaches to regulating how much we should spawn of each category -

1) Calculating how much we need of each category based on expectations<br>
2) Observing and reacting to the real game conditions

Both of those approaches have advantages, so I think the optimal approach is to do both! That is, I think we should keep a target value in memory for each category and adjust it in two different ways -

1) Whenever we make a change to our energy income, we apply an adjustment to our target values for the other categories based on our calculated expectations.<br>
2) Whenever we observe a surplus or shortage of a category, we apply an adjustment to the target values based on that observation.

With that combined approach, we end up with a target that is close to optimal after every adjustment to our income (from our calculated expectation), but we also have the reactive system to hone in on exactly the right value for the real game conditions.

<h4>{{ "Regulation (Miners and Reservers)" }}</h4>

This category is different, because it affects the entire scale of our economy. We increase it when we think we can handle more, and decrease it when we think we can't handle as much. This can be constrained by either spawn time or CPU (or we could drop remotes due to danger). Another thing of note is that changing the reservation status of a remote room is a change to energy income, and should be treated accordingly: The targets for haulers and workers should react, and if the change puts us beyond our constraints, we should scale back to stay inside our constraints.

<h2>{{ "Spawning Priorities" }}</h2>

<h4>{{ "Default Priorities" }}</h4>

Once we know what total creeps we want to spawn, we need to spawn them in an efficient order. In general, the priority order goes:

Income -> Transportation -> Spending

That should be the default order, all else being equal, but there are exceptions to that. Before we get to the exceptions, I will explain why that is the default order.

Income comes first, because the whole of our economy depends on having the energy get mined, and because the energy can build up and get used later (at the cost of decay sometimes, but if you wouldn't have mined that energy at all otherwise, you would have lost more by not mining than you'd lose to decay). In fact, the priority of energy income is so high that it's beneficial to spawn it early in some cases. More on that later.

Transportation comes next because you need it to get the energy to useful places, including more spawning. If you're ramping up spawning, then spawning will be a higher percentage of your income so you might not need workers at all until you've ramped up.

Spending is last because you only need it if you have enough income and transportation to cover your spawning needs and then some.

<h4>{{ "Exceptions" }}</h4>

When you are just booting up your economy and are lacking in haulers, then haulers become a higher priority than miners so you can get enough energy back to the spawn to keep the spawning going. Once you have enough haulers to feed the spawn (or the haulers are waiting on energy from the miners), miners become higher priority again, and remain so most of the time.

If haulers are standing around with energy and have nowhere to put it, then worker spawning becomes higher priority than hauler spawning, so that the haulers have somewhere to drop the energy productively. Once haulers have dropped off all their energy, then haulers become higher priority again.

<h4>{{ "Miner Pre-Spawning" }}</h4>

You don't want to lose any mining time, since any mining time you lose is wasted energy. For that purpose, you always want to spawn a miner early enough that it will have time to travel to its source. And if you have to move that time to fit in more spawn uptime, you want to move it forward rather than back. For that reason, when doing miner spawning you should keep track of when the next scheduled miner spawn is, and if your spawning priorities dictate that the next creep you spawn would come out after that scheduled spawning time for the next miner, spawn the next miner now instead (earlier than scheduled). That will waste some miner TTL, but it will be less waste than you would lose by having the source un-mined for any amount of time.

<h4>{{ "Sub-Category Priorities" }}</h4>

There can be multiple creep types in each category, of course. So I'll say a bit about the priorities within each category.

Within the income category, the main thing is to prioritize closer sources earlier, since energy closer to you is more valuable than energy far away. Where reservers fall into the priority is less important, but I spawn them prior to the miners for each source, since their spawn time is so fast anyways.

Within the transportation category, ideally all the creeps should be the same. If you have separate hualers for different roles (like remote haulers) I would advise eliminating that distinction. Although, in the meantime, the best way to handle it if you have separate remote haulers would probably be to lump the remote haulers for each source into the income category instead. It's beyond the scope of this post, in any case. :P

Within the worker category, the top priority is a tiny 1-work upgrader if your controller downgrade timer is low. The next priority is a repairer if you have structures in need of repairs. Next, spawn builders if there are things to build. Next, spawn additional repairers if you want to build up ramparts. And finally, spawn upgraders to use the rest of the energy.