---
layout: post
title:  "Creating, Connecting, and Populating a MySQL Database Using AWS RDS"
date:   2021-05-26 19:05:20 -0500
comments: true
categories: blog
excerpt_separator: <!--more-->
---

This is how I was able to use AWS Free Tier Services and MySQL Workbench to put together an AWS RDS (Relational Database Service) DB Instance on a virtual private cloud (VPC).
<!--more-->

**RDS, VPC and DB Instances**

Amazon RDS is a service provided by AWS that allows you to automatically set up, operate, and scale relational databases in the cloud. This service covers a lot of the tedious IT infrastructure work associated with developing your own databases on private servers like hardware provisioning, database setup, patching and backups so that AWS RDS users can focus on application development.

You can run a DB instance on a virtual private cloud (VPC) using the Amazon Virtual Private Cloud (Amazon VPC) service. When you use a VPC, you have control over your virtual networking environment. You can choose your own IP address range, create subnets, and configure routing and access control lists.
