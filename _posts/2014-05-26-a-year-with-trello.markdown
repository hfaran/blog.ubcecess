---
layout: post
title: A Year With Trello
author: Hamza Faran
---

Let’s face it; we do some awesome stuff. More specifically, we plan awesome events. Big, awesome events. Like, say, our (hopefully annual continually) Silicon Valley trip.

Projects like these take lots of planning. Organization. Coordination. Synergy (joking, kind of). But seriously, these things need good project management strategies, and the tools to enact.

We tried Trello for this purpose last year. Was it satisfactory? Sure. Could we do better? Let's find out.

## Breaking Down the Problem (That is, Failing to)

Case study time! Let's look at the Silicon Valley board from last year on Trello.

![Silicon Valley Trip 2013/2014 Planning Board](https://gist.githubusercontent.com/hfaran/e0a9a1d8b9978bbb1414/raw/dc7c2c596a8ec3154f26b11f0ed8bf964aca9d71/si_valley_board.png)

Trello offers us a nice and simple Kanban board. The board UI is admittedly very easy-to-use and really easy to get started with. We even get a way to represent subtasks in cards via checklists.

And unfortunately, those are all of the good things I have to say about Trello. As for what follows...

### *What am I supposed to be doing? What are you supposed to be doing?*

I'm not going to espouse any particular named project/team management methodology, but I'm going to lay out what I consider are the components of a successful one.

A project such as the Silicon Valley trip consists of a lot of work, to be done among lots of people. It only makes sense that we break this down into smaller chunks of work, and continue to break this down until we get small bite-sized chunks for a **single** person to do at a time. The culmination of these efforts by each individual make the event happen.

Trello lets us break the large problem of Silicon Valley down into smaller problems, i.e., cards. And that is it. Just the one layer. What often happens is that we have multiple people per card, who are to work on chunks of that same task. At that point:

* The task definition is still nebulous
* Because the task definition is nebulous, an attempt may be made to further define the task as a checklist (subtasks)
* Either way, members of that card do not necessarily know what portion of the task they are assigned to
* Even if this data is encoded in the checklist, there is no way to know this without actually opening up the card

**TL;DR: There are no clear per-member assignments because members are all attached to one ticket (and checklists don't help here). We need real subtasks.**

## Death by a Thousand (okay, four, at least) Cuts

### (Lack of) Closure

*I just finished whatever task was defined in a card. Woo! Now what? Do I archive it? Hmm, but that way I won't be able to see it in my nice `Done` column (or indeed, really anywhere).*

*Alright, I won't archive it and I'll instead move it to the `Done` column.*

*Well, I did that, but it doesn't really feel like I'm done; that due date that is still on the card is saying it's overdue. And I'm still assigned to the card so it's showing up in my Cards view that I
have an overdue card even though it's in the `Done` column!?? Oh god why??? \*Dies\**

So maybe that was a little dramatic.

Actually, no. It wasn't. That is how I actually feel whenever I have to figure out what to do a Trello card that I'm done with. I did not even have to go inside some rage-filled part of my mind to find the above monologue.

I understand that Fog Creek want to keep Trello as simple as possible (and as close to a real Kanban board as possible). But honestly, it **could** be a little smarter.

**TL;DR: Arggghhhh**

### *What* **am** *I supposed to be working on?*

At any point in time, my Trello home page is filled with tens of boards. That's a lot of boards. I really do dislike checking each and every board to gain indications of what I should be spending my time on at the given moment.

But that is exactly what I have to do.

The `Cards` view (which allows you to sort cards you are a member of by due date as well as by board) is nice, but, its severely hampered by what I talk about in '(Lack Of) Closure'. If you don't keep great track of your due dates and card memberships, have fun attempting to seriously make use of `Cards`.

**TL;DR: =\(**

### *And the context of this card is*?

Linking cards. Dependencies between cards. I realize Fog Creek are probably working towards this, but Trello doesn't have these features **right now**, and it hurts Trello, **right now**.

**TL;DR: =\|**

## *I'm Calm, I'm Calm*

Maybe I am being too hard on Trello. Maybe I'm expecting Trello to be too much. Perhaps I'm using incorrectly, or I'm using it for the wrong kind of projects.

Perhaps all the above is true. But in my whole year of seriously attempting to use Trello, I have not found the perfect use case. Maybe tracking a whole project on a single Kanban board isn't the way to go.

## What Now?

Yeah, this was mostly a rant but I felt it was a productive rant as I tried to highlight the biggest pain points of Trello so that we can work towards something that alleviates those pain points.

We can try and make Trello (or our usage of it) itself better ([Tiana has a nice hack with linking cards as subtasks](https://trello.com/b/Ros7vhiz/may-roadmap-spm1)).

Or we can consider replacing Trello. I will be looking into further options.

Will we find the the PERFECT system? HAH! No. But I'm at least hoping we can improve upon what we had last year by picking something that improves upon my four points listed.
