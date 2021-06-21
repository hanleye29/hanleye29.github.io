---
layout: post
title:  "AWS Cloud Practitioner Certification"
date:   2021-06-20 00:00:00 -0500
comments: true
categories: blog
excerpt_separator: <!--more-->
---

*What is Cloud Computing? What is AWS? Why is everybody so excited about it? This post covers a little bit of what I learned on my journey to becoming a certified AWS Cloud Practitioner.*

<!--more-->

<center><img src="https://1x5o5mujiug388ttap1p8s17-wpengine.netdna-ssl.com/wp-content/uploads/2020/12/AWS-logo-2.jpg?_ga=2.174531175.492004798.1624295253-1397733696.1624295253" style="height: 300px; width:400px;"/></center>

I was talking with an established Data Science Manager at my company about what I could do to improve my data science skillset. His advice was to achieve my AWS Cloud Practitioner Certification. Up to this point, I knew that Amazon Redshift was a popular data warehousing solution used by a number of businesses, but I really wasn't familiar with AWS outside of that - or cloud computing in general.

What I found was absolutely astounding! You always hear about Amazon being this goliath entity - I'd always associated them with the nice books or trinkets or dog costumes I order that arrive in my doorstep just a few hours later. But even with how impressive their resale and distribution segments (Amazon North America/International) are - AWS has convinced me that Amazon will probably end up ruling the world. That's because AWS owns **32% of the Cloud Infrastructure Provider Market** - which is a $130 Billion market.

Alright cool, but what's Cloud Infrastucture? According to AWS:

> Cloud computing is the on-demand delivery of IT resources over the Internet with pay-as-you-go pricing. Instead of buying, owning, and maintaining physical data centers and servers, you can access technology services, such as computing power, storage, and databases, on an as-needed basis from a cloud provider like Amazon Web Services (AWS).

Let's break that down a little - how can you access technology services like the ones described above without buying, owning, and maintaining physical data centers and servers? Well, Amazon already owns a ton of them. And they're everywhere.

## AWS Regions & Availability Zones
<center><img src="https://d1.awsstatic.com/about-aws/Global%20Infrastructure/NA-500x500.f8738d3a3341a06a83fa838b927ba4b85b473918.png" style="height: 400px; width:400px;"/></center>

The above image only shows you North American **Regions**. Again from AWS:

> AWS has the concept of a Region, which is a physical location around the world where we cluster data centers. We call each group of logical data centers an Availability Zone. Each AWS Region consists of multiple, isolated, and physically separate AZs within a geographic area. Unlike other cloud providers, who often define a region as a single data center, the multiple AZ design of every AWS Region offers advantages for customers. Each AZ has independent power, cooling, and physical security and is connected via redundant, ultra-low-latency networks. AWS customers focused on high availability can design their applications to run in multiple AZs to achieve even greater fault-tolerance. AWS infrastructure Regions meet the highest levels of security, compliance, and data protection.

So these Availability Zones each contain servers upon servers upon servers. AWS offers **Virtual Machines (VM)** that can run on these servers as software, but function like a regular server. Multiple VM's can run on the same physical server, a concept known as multitenancy, and these VM's are isolated and have resources allocated to them by a program called a **Hypervisor**.

## Amazon Elastic Compute Cloud (EC2)
To clarify this a bit - let's look at one of AWS's main products - the **Elastic Compute Cloud (EC2)**. You can go online and start one of these virtual machines at any availability zone in any region with the click of a few buttons. As you can see in the image below, when the EC2 instance is made, it is created on a host server that may already have several other EC2 instances associated with it. All of the instances are isolated and hardware resources are allocated by the hypervisor. This EC2 instance can be configured with your choice of processor, storage, networking, operating system, and purchase model, and you can immediately begin running workloads off of it. So literally with the press of a few buttons and a few minutes - you can have a super powerful computing instance up and running anywhere in the world.

<center><img src="https://image.slidesharecdn.com/cmp402-151007210659-lva1-app6891/95/cmp402-amazon-ec2-instances-deep-dive-3-638.jpg?cb=1444253756" style="height: 400px; width:600px;"/></center>
