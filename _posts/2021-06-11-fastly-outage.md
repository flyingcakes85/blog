---
layout: post
title: Fastly CDN Outage
subtitle: A global outage fixed in time
categories: internet
banner: https://images.pexels.com/photos/2881229/pexels-photo-2881229.jpeg?auto=compress&cs=tinysrgb&h=750&w=1260
tags: internet
---

June 8 - I deployed a simple webpage on GitHub pages. I shared it on a forum where users will take interest in the site. Very soon, a user shared a screenshot saying that the website was inaccessible. At first I was confused as to why this happened. My site was a simple one page HTML site. No external dependencies or sources (not even fonts).

I quickly tried accessing my webpage and found that indeed my site was giving a 503. I looked up if GitHub was down. This led me to a tweet by GitHub where they acknowledged that their services were experiencing problems. Users started reporting that fetching assets from GitHub resulted in errors. Among many other things, this affected installation process of Linux-based distros which relied on GitHub to host repositories, like ArchCraft.

That afternoon I couldn't access Reddit. Their services were down too. It seemed strange that two big websites were experiencing issues at the same time. Reading up on news sites, I found that Fastly CDN was facing an outage that led to many other services being down too - Twitch, Amazon, Spotify, HBO Max, Quora, PayPal, Vimeo, the New York Times, BBC, Financial Times etc.

Good news is that the outage was not a result of malicious intentions of a hacker. Services started being restored within 8 hours.

CDN outages can mean significant inconvenience. Smaller websites don't need to rely on CDN. These are of prime interest to those services that expect huge traffic and it will be virtually impossible for a single server to handle all the requests. CDN help in this situation by distributing load across different servers around the world. When CDNs go down, they defeat their purpose. However such outages don't happen every now and then. Usually a specific region faces outage and the traffic in that region is redirected to a different region. This way, the global network of CDN ensure that outage at a particular server location doesn't affect services. This time unfortunately, this was a global outage where majority regions were affected.
