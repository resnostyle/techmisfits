---
layout: post
title: "Docker +  Jenkins"  
date: 2016-05-01 20:01:01   
categories: jenkins docker 
---
In terms of maintaining Jenkins slaves increasing loads on CI infrastrucutre using Docker containers has a been a solid win.  

Using a configuration management tool we spin up our slaves with a bare bones set of packages to get Jenkins connected, running and authenicated our Docker registry. Next we use docker-compose to link and bring up the containers required into a desired state. Lastly, with those containers running we execute out testing suite. 
