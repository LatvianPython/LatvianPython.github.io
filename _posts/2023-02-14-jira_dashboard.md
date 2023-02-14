---
layout: post
quote: Custom Jira dashboard I made 🐍
title: Jira Dashboard
---

Implementation at [GitHub](https://github.com/LatvianPython/jira_dashboard)


<br>

I kinda already write a little about the situation in the GitHub README.md
 , so as not to repeat myself, in here is mostly some backstory that I did not feel like 
belonged in the GitHub README.md, also the post can just serve as a way to point that
 the GitHub repository exists

<br>

So, some backstory then. In the project I worked at, we used to have a dashboard for maintenance issue SLA, 
and at some point, the dashboard ([Only reference I can find of it today](https://twitter.com/sladiator)) "SLAdiator"
was eventually put out to pasture, so the dashboard me and other people in the project relied on
to check the SLA status of maintenance issues was no longer there.

<br> 

Due to having too much time on my hand, I set out to create my own version, (Insert Bender from Futurama quote here).
Which was also my own little project on how to create web-sites. The project went through many iterations
where I tried out different technologies, Python packages, web frameworks. I tried [CherryPy](https://docs.cherrypy.dev/en/latest/),
[Django](https://www.djangoproject.com/), and in the end [Flask](https://flask.palletsprojects.com/) + [Vue.js](https://vuejs.org/).
Each time I reimplemented the solution, I added more functionality to the dashboard. 

<br> 

In the end, the version I settled with was Flask + Vue.js, which is the one I published on GitHub.
