# Kubernetes

## Outline
Good morning everyone, thanks for joining. Today, I will be going over Kubernetes. The goal here
is to demystify some of this technology by giving an interactive demo of the technology and
what it offers.
I think it's important for everyone to understand what this technology is and how it works.
Kubernetes forms the basis of what is the cloud, as we know it. Every major cloud; google, aws,  azure
openshift, etc. They all run on top of kubernetes. They both incapsulate as well as extend the
essential functionality that kubernetes provides.

<-- Kubernetes history -->

So today basically I'm going to take you through a step by step process of how this all comes together
from the ground up. The agenda is: a brief history, which we just went through, and then we'll
start by creating a very simple application, and walking you through step by step how this would
be orchestrated in a kubernetes environment. I'm going to start basic and build up on it because
to truly understand how cloud orchestration works, there are some very basic and fundamental things
that you need to understand about how applications work in a cloud environments. So we'll go through a
whole example, and i hope the demo gods bless me, i've put tanner up as my ritual sacrifice, and
then i have a cool project to show off to further explain how all this stuff works. I will be
coding through parts of this, but i have tried to keep this as simple as i possibly can. You can
ignore a lot of it, ive got it all copy and pasted and ready, I'll point out the important bits
so just try and i think everyone here should be able to follow along.

So where do we begin? I'm going to start with a simple application. All computer have ports, as we
know. <-- picture -->. and when we run services, those services listen on those ports. and when
then are listening, other computers can now communicate with them. they do this by specifying 
the name (or ip) and the port number. so let's do this, let's create a simple program and run
it on my computer.

<-- write program -->
[setup
  dual - terminal,finder
  path - nested folder
  node
]

so here's an empty folder. and i'm largely going to copy over the necessary files from this other
folder <-- copy command --> and let's open up the main index file.
We're going to create a webapp listening on port 92



1. Docker - what is a container
2. Kubernetes - what is a pod
