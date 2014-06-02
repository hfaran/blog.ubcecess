---
layout: post
title: A Somewhat Objective Look at Facebook Course Groups
---

## Why?

Denise asked the following question in the Year Rep transition docs regarding
Facebook course groups:
*Did they work? How many students were in them? Was it active?*

I thought I'd take some time to answer this properly. Like, **properly**
properly. So let's get started.

## Getting the Data

It's probably no secret that I like Facebook groups for things. I do absolutely
hate how simple and lacking of functionality they are, and how much better they
could be; but everyone has Facebook and it's simple and it just works **good
enough** for something like course groups for classes.

So anyways, I made lots of course groups this year. **Lots**. I also made a
script to print out a "nicely" formatted list of the course groups I made this
term, [available here](https://www.dropbox.com/s/p76h2pdfp1jvnl8/courses.py).
Let's grab the weird dictionary of courses from that list and get started.

I'll only be looking at course groups from **2013W Term 2** (with some data that
is baggage from Term 1 for which there is a caveat later).

{% highlight python linenos %}
from courses import COURSES
{% endhighlight %}

Now, to analyze any data about these groups, I first need to get said data from
Facebook's Graph API. To do **that**, I need to first get the `group-id` of each
group. First, let's make a long and complicated list comprehension to flatten
the above monstrosity.

{% highlight python linenos %}
courses = [
    "{} {} - Term 2 2013W".format(
        dept,
        course[0] if isinstance(course, list) else course
    )
    for dept, dept_dict in COURSES.items()
    for level, course_list in dept_dict.items()
    for course in course_list
]
{% endhighlight %}

{% highlight python linenos %}
courses
{% endhighlight %}



    ['EECE 300 - Term 2 2013W',
     'EECE 310 - Term 2 2013W',
     'EECE 315 - Term 2 2013W',
     'EECE 320 - Term 2 2013W',
     'EECE 331 - Term 2 2013W',
     'EECE 352 - Term 2 2013W',
     'EECE 353 - Term 2 2013W',
     'EECE 356 - Term 2 2013W',
     'EECE 359 - Term 2 2013W',
     'EECE 360 - Term 2 2013W',
     'EECE 364 - Term 2 2013W',
     'EECE 373 - Term 2 2013W',
     'EECE 380 - Term 2 2013W',
     'EECE 381 - Term 2 2013W',
     'EECE 392 - Term 2 2013W',
     'CPSC 304 - Term 2 2013W']




### Finding `group-id`s

Neat. Now that we have that list, we can feed to search requests to the Graph
API which will give us the IDs of each group so that we can look each group
individually later.

{% highlight python linenos %}
import facebook

# OAuth token redacted for obvious reasons
graph = facebook.GraphAPI(FB_OAUTH_TOKEN)
course_ids = [graph.request(
    "search",
    args={
        "q": group_name,
        "type": "group"
    }
)["data"][0] for group_name in courses]
{% endhighlight %}

### Getting Group Members and Posts (i.e., the Actual Data)

Let's populate `course_ids` with some more information. Since we want to gauge
activity of these groups, we can grab all posts on each from `/group-id/feed`
and a list of all members from `/group-id/members`.

{% highlight python linenos %}
for d in course_ids:
    d["members"] = graph.request(
        "/{id}/members".format(id=d["id"]),
        args={"limit": 500}
    )
    d["feed"] = graph.request(
        "/{id}/feed".format(id=d["id"]),
        args={"limit": 500}
    )
{% endhighlight %}

## "Analysis"

We are now dealing with quite a large dataset so I'll refrain from printing it
out (the above took many legitimate seconds to actually complete). But let's
start by looking at some basic numbers:

{% highlight python linenos %}
course_stats = [{
    "name": d["name"],
    "num_members": len(d["members"]["data"]),
    "num_posts": len(d["feed"]["data"])
} for d in course_ids]
{% endhighlight %}

{% highlight python linenos %}
course_stats
{% endhighlight %}


    [{'name': u'EECE 300 - Term 2 2013W', 'num_members': 13, 'num_posts': 11},
     {'name': u'EECE 310 - Term 2 2013W', 'num_members': 26, 'num_posts': 1},
     {'name': u'EECE 315 - Term 2 2013W', 'num_members': 57, 'num_posts': 17},
     {'name': u'EECE 320 - Term 2 2013W', 'num_members': 80, 'num_posts': 18},
     {'name': u'EECE 331 - Term 2 2013W', 'num_members': 11, 'num_posts': 7},
     {'name': u'EECE 352 - Term 2 2013W', 'num_members': 66, 'num_posts': 54},
     {'name': u'EECE 353 - Term 2 2013W', 'num_members': 113, 'num_posts': 36},
     {'name': u'EECE 356 - Term 2 2013W', 'num_members': 91, 'num_posts': 98},
     {'name': u'EECE 359 - Term 2 2013W', 'num_members': 60, 'num_posts': 43},
     {'name': u'EECE 360 - Term 2 2013W', 'num_members': 74, 'num_posts': 25},
     {'name': u'EECE 364 - Term 2 2013W', 'num_members': 85, 'num_posts': 47},
     {'name': u'EECE 373 - Term 2 2013W', 'num_members': 81, 'num_posts': 82},
     {'name': u'EECE 380 - Term 2 2013W', 'num_members': 107, 'num_posts': 44},
     {'name': u'EECE 381 - Term 2 2013W', 'num_members': 49, 'num_posts': 6},
     {'name': u'EECE 392 - Term 2 2013W', 'num_members': 28, 'num_posts': 4},
     {'name': u'CPSC 304 - Term 2 2013W', 'num_members': 21, 'num_posts': 2}]


Now that we have the numbers, let's play with them a bit/see how they look on
some graphs.

{% highlight python linenos %}
data = sorted(course_stats, key=lambda x: x["num_members"])
{% endhighlight %}

{% highlight python linenos %}
print("Average # of members: {:.0f}".format(
    average([i["num_members"] for i in data])
))
print("Average # of posts: {:.0f}".format(
    average([i["num_posts"] for i in data])
))
{% endhighlight %}


    Average # of members: 60
    Average # of posts: 31



Before I start talking about this at all, **HUGE CAVEAT**: **some of these
groups were re-used from Term 1**, so the numbers we are looking aren't only
from this term (so they are inflated if we're only considering Term 2). Do keep
this in mind.

That being said, these averages look really promising. Consider your average
class and its size. Now look at the average number of members in course groups.
`60` is a solid number here (at least in my book), even if this number is
inflated. So we definitely have students **joining** these groups.

Now what about participating? Well, `31` may not be a huge number, but even so,
`31` suggests usage on a notable level. People are asking questions, providing
answers, and useful materials. We'll investigate this more later.

{% highlight python linenos %}
# Here's a plot of the numbers, make of it what you will
pyplot.plot(
    [c["num_members"] for c in data],
    [c["num_posts"] for c in data],
    'g-',
    linewidth=3,
)
xlabel("Number of members")
ylabel("Number of posts")
title("Number of Posts in Groups vs. Number of Members")
{% endhighlight %}

![png](/images/fb_course_group_analysis_23_1.png)


### Enough Numbers, Let's Talk Words

There's **lots** of text available in the `feed` data of each group. Mostly
(well, exactly) all in posts and comments. Let's flatten all of those scattered
strings into a single gargantuan list.

{% highlight python linenos %}
from itertools import chain

all_text = list(chain(*
    # Grab body of each post
    [[post["message"].lower()] if "message" in post else [] +
     [comment["message"].lower() for comment in post["comments"]["data"]]
     if "comments" in post else []  # And then grab all of its comments
     for course in course_ids for post in course["feed"]["data"]]
))
{% endhighlight %}

{% highlight python linenos %}
# How many text posts and comments? (Also, I lied in the title,
# there are definitely numbers in this section >:D)
len(all_text)
{% endhighlight %}



    493



{% highlight python linenos %}
# How about WORDS?
all_words = list(chain(*[text.split() for text in all_text]))
len(all_words)
{% endhighlight %}




    10210



Let's start off with a [wordcloud](https://github.com/amueller/word_cloud),
because those are always fun.

{% highlight python linenos %}
import wordcloud
from IPython.core.display import Image

dimensions = dict(width=728, height=364)

# Monkey-patch FONT_PATH
wordcloud.FONT_PATH = "C:\\Windows\\Fonts\\trebuc.ttf"
# Separate into a list of (word, frequency).
words = wordcloud.process_text(" ".join(all_text), max_features=500)
# Compute the position of the words.
elements = wordcloud.fit_words(words, **dimensions)
# Draw the positioned words
wordcloud.draw(elements, 'wordcloud.png', **dimensions)
Image(filename='wordcloud.png')
{% endhighlight %}


![png](/images/fb_course_group_analysis_30_0.png)


Does anyone know, indeed...

Let's look at a table of the 30 top interesting words. Note: I have filtered out
numbers from the following table (they were very popular but ultimately
uninteresting).

{% highlight python linenos %}
from prettytable import PrettyTable

t = PrettyTable(["Word", "Frequency"])
for w, f in filter(lambda w: w[0].isalpha(), words)[:30]:
    t.add_row([w, "{:0.3f}".format(f)])
print(t)
{% endhighlight %}

    +------------+-----------+
    |    Word    | Frequency |
    +------------+-----------+
    |   anyone   |   1.000   |
    |    know    |   0.532   |
    |  question  |   0.314   |
    |   class    |   0.295   |
    |    lab     |   0.288   |
    |    guys    |   0.269   |
    |    part    |   0.263   |
    |    hey     |   0.263   |
    |    set     |   0.250   |
    |   thanks   |   0.218   |
    |  midterm   |   0.199   |
    |  problem   |   0.192   |
    |    one     |   0.167   |
    |    use     |   0.167   |
    |    need    |   0.167   |
    |    will    |   0.167   |
    |   please   |   0.160   |
    |   right    |   0.154   |
    |  someone   |   0.154   |
    |  project   |   0.147   |
    |   notes    |   0.141   |
    | assignment |   0.141   |
    |  tomorrow  |   0.135   |
    |    week    |   0.135   |
    |    find    |   0.135   |
    |    due     |   0.135   |
    | questions  |   0.135   |
    |     e      |   0.128   |
    |  connect   |   0.122   |
    |   really   |   0.122   |
    +------------+-----------+


This list and wordcloud is very encouraging to me. The discourse seems to be
mostly pleasant and helpful, which goes along with my anecdotal observations of
the discourse on these groups.

Also, there is a surprising lack of expletives overall, one that I completely
did not expect.

{% highlight python linenos %}
[w for w in all_words if w in ["shit", "damn", "fuck", "crap", "bull",
"bullshit", "bs"]]
{% endhighlight %}



    [u'shit', u'shit']



SEE WHAT I MEAN!? **That** is the level of civility on these groups. Completely
unprecedented.

## Sudden Conclusion

I would go further with this, but without more techniques for meaningful
analysis of the we have, I cannot say much more on this subject of substance.

Also, I have been working on this for too long and would now like to watch TV.

Suffice it to say, students are using these groups, and we should keep going
with them (i.e., making new course groups on Facebook). **However**, this data
should be looked on again so that we may potentially learn more and improve on
Facebook course groups in anyway possible.
