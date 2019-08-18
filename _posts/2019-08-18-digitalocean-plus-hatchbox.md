---
title: DigitalOcean + HatchBox
excerpt: HatchBox/DigitalOcean is a great combination to launch products on. Whether prototypes or client demos. But it’s also strong enough to get a Rails-based venture into profitability.
---

 ![]({{ site.url }}/assets/postimages/dohb.png)

I’ve been hosting my Rails projects on [Heroku](https://www.heroku.com/){:target="_blank"} for years now. It’s a great service for a developer because it insulates you from a lot of the IT/Operations issues involved with deploying and managing a Rails app. But it can get expensive, especially when you want to start layering on background processing and multiple instances. Overall Heroku has been good to me over the years but I’ve been looking for something that was just as convenient but more affordable and bit more flexible.

A lot of solutions which offer “1 click deploy” like Heroku rely on technologies such as Docker. And these are fine, but they’re also heavy and I find them slow to spool up and to deploy. Plus I really didn’t want to get saddled with yet another technology suite I would be forced to learn.

Then I came across a Reddit post talking about a service called [HatchBox](https://www.hatchbox.io/){:target="_blank"} by Chris Oliver from GoRails. He describes the system as: _"Now, Hatchbox isn't designed to replace Heroku or anything like that. If you want truly hands-off, you'll have to pay for Heroku or something similar. If you want something cheap and easy to use, but don't mind running your own servers, then Hatchbox will hopefully be perfect for you.”_ Which sounded like exactly what I wanted. Any big projects will eventually be moved to [Engine Yard](https://www.engineyard.com/){:target="_blank"} as I used them for many years at an enterprise-level start-up. But for growing a site to that point, HatchBox sounded perfect.

HatchBox prefers [DigitalOcean](https://www.digitalocean.com/){:target="_blank"} as a services provider which was kind of OK with me as I find Google Cloud woefully slow and confusing to configure, and I find AWS completely Byzantine to try to get things done in. DigitalOcean, by contrast, is very point-and-click. You don’t need to jump through dozens of screens to get something ready to roll. Provisioning is also much, much faster.

HatchBox’s lowest tier costs $29/month to manage two DigitalOcean instances/clusters. The first step is to connect HatchBox to your Github/Gitlab accounts and then to your DigitalOcean account. This sets up your ecosystem to allow you to create servers and deploy apps onto them. 

Creating servers/clusters and putting apps on them is a one-page form for each action. It’s all very well laid out and doesn’t confuse the user with tons of options. Same with deploying the apps. One page and done. There’s a “Deploy” button once you have things set up and one click is all it takes to deploy right from Git.

| ![]({{ site.url }}/assets/postimages/hbdo-2.png) | 
|:--:| 
| *Creating an instance on Hatchbox* |

| ![]({{ site.url }}/assets/postimages/hbdo-1.png) | 
|:--:| 
| *Creating an app on Hatchbox* |


If you install a simple callback hook, you can have your deploys trigger a deploy automagically. This isn’t as thorough as a Continuous Integration platform, but for a solo developer or small team where you just want to Get Things Done, it’s tough to beat.

Log access is all available via the HatchBox web app, and a console is available from the DigitalOcean web app, so as a platform you get all the convenience you would find in Heroku. 

DigitalOcean has really upped their game of late, making the move even easier to justify. They now offer something called “[Spaces](https://www.digitalocean.com/products/spaces/){:target="_blank"}” which is an AWS/S3 compliant CDN storage system. Basically for $5/mo you get everything you need but hosted on DO’s servers. So you don’t need to hassle with the AWS console at all if you go this route. 

This also ties into database backups. Hatchbox provides integration with the Backup gem as a one-click option. Click the button and it adds Backup to your instance and hooks it up into cron. A few minutes of editing config files and you can have nightly database back-ups deposited in your Spaces account (so they’re off the server) and you get notified over your Slack channel. Not quite as seamless as Heroku, but it gets the job done. Note that DigitalOcean now also offers [Managed Databases](https://www.digitalocean.com/products/managed-databases/), so as your deployment grows you can easily migrate to a proper enterprise database with back-ups, optimizers, and so on. This is currently only available for PostgreSQL.

Overall I’ve been happy with the performance. Deploys are quick and the servers seem quite snappy. Not being hobbled by Heroku’s pricing I can now deploy things like Redis and Elasticsearch on my own server. Also, Hatchbox makes it easy to deploy multiple apps on one server - so you can run a primary app and then have a few prototypes or demos also running on the same instance. Very handy.

The final kicker was a new developer support network that DigitalOcean created called “[Hatch](https://www.digitalocean.com/hatch/){:target="_blank"}”. No kidding ... can’t beat the symmetry here. If you sign up and are approved for Hatch you get wired into their network and can get hosting credits. I’m just a solo developer and they accepted me and gave me $1000 in credits for the next year. That’s enough to let me move everything off Heroku and used the saved money for marketing my products. A really sweet deal.

HatchBox/DigitalOcean is a great combination to launch products on. Whether prototypes or client demos. But it’s also strong enough to get a Rails-based venture into profitability. By the time you need to worry about the kinds of issues that AWS can help with, you’ll be making enough money to hire an operations expert who might actually enjoy tinkering with it endlessly.
