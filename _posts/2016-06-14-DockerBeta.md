---
layout: post
title: Docker Beta thoughts
---

So I've been using Docker Beta for Mac for about 2 months now and I think I'm finally starting to get some good use out of it.  The ramp up seemed a little harder that I thought it might. 

Setup
---
The setup was super easy, and doesn't really step on anything you already have.  You can't run the old docker machine and the new Docker Beta at the same time, but you can easily switch between the two.  The setup for Docker Beta is definitely a plus!  I love that the the Docker whale icon is up in the top menu now, so you can easily see the status of the environment with a quick click of the mouse.  Another huge plus is you don't have to have a special terminal open for the docker machine to engage with.  Any terminal now knows about the docker commands and it makes switching from checking files into git to building/running a docker container.

Containers
---
Here's where things start to break down.  In the old setup, a container's IP/address was pretty easy and consistent to get to.  Not so much with the new Docker Beta.  If I want to access the Consul container via the browser, I'm connecting to http://localhost:8500.  But if I'm running a container and I need that container to talk to the same Consul instance, I have to use the address http://192.168.65.2:8500.  That took a long while to figure out, as the documentation still references "docker.local" in many places but the forums all talk about docker.local being deprecated and localhost being the address to use for containers.  But the reality seems to be that depending on your setup, the above values may change.  Docker.local certainly isn't an option any more, but depending on your settings via pinata (more on that below), the references for localhost could flip, so that containers know about localhost but then your Mac does not.  

Pinata
---
A semi-documented feature that seems to control networking and other configuration options for your docker environment.  The command line interface is good (and there's no gui at all so that's a plus), but I'm not sure I would have known about pinata if I hadn't been digging around the forums for information on why I couldn't connect containers to localhost anymore.  
