---
layout: post
title: How can we evaluate and assign value to NFL tackles?
subtitle: My submission to the 2024 NFL Big Data Bowl 
gh-repo: BenjaminMillman/2024_BigDataBowl
gh-badge: [star, fork, follow]
tags: [NFL, Football, Statistics, ML]
comments: true
author: Ben Millman
---

{: .box-success}
This project was created as part of a Kaggle competition whose expiration has passed. All work was completed by myself using data from the 2022 season provided by the NFL in a kaggle notebook running R. This was an awesome opportunity to learn and combine my passions as an athlete and data analytics student. I hope you enjoy!

## Introduction
Football can be complex at times, but when it's all boiled down, there's 2 goals. The offense wants to move the ball forward, and the defense wants to stop them. Defenses do this through tackling, and that was the them of this year's competition.  The task at the outset was general - create metrics that assign value to tackling. As someone who has played football my entire life, this seemed simple at first as we sort of intuitively assign value to tackles as we watch, but creating math-driven, objective assesments is much more difficult. I split my approach and focussed on two avenues. First, I wanted to calculate the probability of a player making a tackle as a function of their distance to the ball. Second, I wanted to predict the location of each tackle. With each of these I could generate further metrics. 

## Data Cleaning
Before I could start on any of that, I had to prepare the data. The NFL provided multiple tables including the following: a games table which contained basic information about the game and its final score, a players table which contained incormation about each player and their background, a plays table which detailed the state of the game before and after the play and contained information about the play itself, a tackles table which contained information about every tackle tackle that occured, and lastly, 9 weeks of tracking information containing the football's and every player's positional information of every frame of every play. Each of these also contained various ID columns which could be used to join them. 

For each of these tables I made sure that columns read in as characters that needed to be factors were appropriately converted, and made sure all dates and times were read in correctly or converted to the same format. Next, I combined all of the tracking weeks into one large table. 

[This is a link to a different site](https://deanattali.com/) and [this is a link to a section inside this page](#local-urls).

Here's a table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |

How about a yummy crepe?

![Crepe](https://beautifuljekyll.com/assets/img/crepe.jpg)

It can also be centered!

![Crepe](https://beautifuljekyll.com/assets/img/crepe.jpg){: .mx-auto.d-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.

## Local URLs in project sites {#local-urls}

When hosting a *project site* on GitHub Pages (for example, `https://USERNAME.github.io/MyProject`), URLs that begin with `/` and refer to local files may not work correctly due to how the root URL (`/`) is interpreted by GitHub Pages. You can read more about it [in the FAQ](https://beautifuljekyll.com/faq/#links-in-project-page). To demonstrate the issue, the following local image will be broken **if your site is a project site:**

![Crepe](/assets/img/crepe.jpg)

If the above image is broken, then you'll need to follow the instructions [in the FAQ](https://beautifuljekyll.com/faq/#links-in-project-page). Here is proof that it can be fixed:

![Crepe]({{ '/assets/img/crepe.jpg' | relative_url }})
