---
layout: post
title: "Performance Modeling for Network Control Plane Systems"
date: 2014-10-01 21:51:21 -0700
comments: true
categories: 
---

At Berkeley I have the opportunity to work with some of the smartest undergrads around. One of the undergrads I work with,
[Andrew Or](https://plus.google.com/109177137524762864782/about), did some neat work on modeling the performance of network control plane systems (e.g. SDN controllers).
He decided to take a once-in-a-lifetime opportunity to join [Databricks](https://databricks.com/) before we got the chance to publish his work, so in his stead I thought
I'd share his work here.

An interactive version of his performance model can be found at this [website](https://www.eecs.berkeley.edu/~rcs/research/convergence_modeling/). Description from the website:

> <p>A key latency metric for network control plane systems is convergence time: the duration between when a change occurs in a network and when the network has converged to an updated configuration that accommodates that change. The faster the convergence time, the better.<p><br />
> 
> <p>Convergence time depends on many variables: latencies between network devices, the number of network devices, the complexity of the replication mechanism used (if any) between controllers, storage latencies, etc. With so many variables it can be difficult to build an intuition for how the variables interact to determine overall convergence time.</p><br />
> 
> <p>The purpose of this tool is to help build that intuition. Based on analytic models of communication complexity for various replication and network update schemes, the tool quantifies convergence times for a given topology and workload. With it, you can answer questions such as "How far will my current approach scale while staying within my SLA?", and "What is the convergence time of my network under a worst-case workload?".</p>


The tool is insightful (e.g.
note the striking difference between SDN controllers and traditional routing protocols) and a lot of fun to play around with; I encourage you to check it out.
In case you are curious about the details of the model or would like to suggest
improvements, the code is available [here](https://github.com/andrewor14/web-model). We also have a 6-page write up of the work, available upon request.
