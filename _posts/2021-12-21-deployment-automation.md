---
layout: post
quote: 📉 over 4 years for production
title: Deployment Automation
---

**tl;dr** - manual bad, automated good

[Skip to plots](#plots)

<br/>
# Introduction
When I started working on the current project at work there was no automation for deployments either for the production
or for the test environments. Each developer was responsible for deploying their code to the test environment. For production,
there was a rotation on who was responsible for deploying code on any particular week. In short, we had to deploy both server-side
and client-side objects, PL/SQL code for the server-side, and Oracle Forms/Reports for the client-side.

<br/>
During the era of manual deployments, we had to do the following:
1. prepare server-side scripts, the developer decides and creates the order of execution for PL/SQL scripts, 
mostly done before deployment in the evening;
1. stop scheduling of new background processes in the database; 
1. deploy changes, connect to production databases and execute scripts for each schema where changes need to be deployed;
1. check databases, ensure no invalid objects, check that scripts are executed without error;
1. compile client-side objects, forms, libraries, and/or reports;
1. deploy client-side objects to servers, and check that they can be opened/used;
1. enable scheduler of background processes, and declare deployment a success (hopefully).

<br/>
All of that work is mostly the same no matter what you deploy. Same actions performed routinely, only in exceptional
cases do developers need to deviate from a set series of actions that could very well be automated.


<br/>
On how I got the data for the plots. We started to log how much effort was being spent on deployments to 
production at the beginning of 2018.
The project did not start in 2018, it is just when developers had to start logging the time spent per deployment. 
Total logged time includes preparations for individual deployments (preparing deployment scripts) and the time
spent in the evening for the actual deployment to production.


<br/>
Originally the person responsible rotated often, then only a single person was assigned long-term for deploying code
to production. I got assigned the responsibility of deploying changes to production in late 2019, where one of
the first things I started doing was automating away my work due to how monotonous and
time-consuming it was. The first attempts to automate the deployment of changes was a success and was
quick to implement. The automatic deployment scripts were written in Python 🐍.

<br/>
At the end of 2020 further improvements were made to the scripts essentially requiring me to simply click a button
and watch all of the code be deployed to the databases, and the client-side be recompiled and deployed to the required
servers.

<br/>
The automation was not only for the production environments but also for the test environments. The time saved 
for deployments to the test server is not as easily representable, as we did not log the time
for deployments on the test environments and so I cannot plot anything for that. 

<br/>
# Plots

I like this plot the best, we can see how things were during the dark times, and what the impact of automation was, a great
victory for my sanity.

![total_mean_time_spent_on_deployments](/assets/plots/deployment_automation/total_mean_time_spent_on_deployments.svg){: width="90%" }

If we want to compare just the mean time spent per deployment, we can see that here:

![mean_time_spent_on_deployments](/assets/plots/deployment_automation/mean_time_spent_on_deployments.svg){: width="90%" }

Or if you would rather prefer to compare the total time spent, see:

![total_time_spent_on_deployments](/assets/plots/deployment_automation/total_time_spent_on_deployments.svg){: width="90%" }


# Conclusions
I would suggest that anyone, anywhere, at least think about automating their workflow, it can save you immense amounts of
time in the long run. Pre automation ~160 hours (1 man-month) were being spent per year for deployments, right now we spend
~40 hours per year. I can say that we have saved at least two months of developer effort with the automation scripts,
this does not include the time that the client spends during the evening deployments, nor does this account for
the time saved for deployments on the test environments.

[![Is It Worth the Time?](https://imgs.xkcd.com/comics/is_it_worth_the_time.png)](https://xkcd.com/1205/){: width="60%" }