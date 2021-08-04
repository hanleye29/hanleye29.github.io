---
layout: post
title:  "Automate API Data Retrieval on Windows Task Scheduler"
date:   2021-08-04 00:00:00 -0500
comments: true
categories: blog
excerpt_separator: <!--more-->
---

*AWS Lambda Functions? GCP Cloud Functions? Who needs them?! Let's take a look at how you can automate a request to an API using your laptop. We'll be using the Twitter API, but you can use any API you'd like for this.*

<!--more-->

*This exercise assumes that you already have your API Key, API Secret Key, API Token, and API Token Secret to access the Twitter API saved in a file called "config.py" within the same directory as your the python script you'll be using to make the request. Also, it should go without saying that you're running windows. If not, find a different tutorial.*

# Overview

First thing's first. What is Windows Task Scheduler? Windows Task Scheduler is a tool that allows you to create and run virtually any task automatically. Rather than sitting there pressing execute over and over and over, Windows Task Scheduler is able to run these tasks for you at prespecified times. Actually, if you open Windows Task Scheduler right now, you'll see applications that have already set up a number of automated tasks. Each task is comprised of General Information, Triggers, Actions, Conditions, and Settings.

## General Information

<center><img src="https://github.com/hanleye29/hanleye29.github.io/blob/main/docs/_includes/General.PNG?raw=true" style="height: 300px; width:400px;"/></center>

What we'll need is the actual python script (in this case, mine is called pull_ten_tweets.py) we'll want to execute that will be requesting data from the API, formatting the data, and storing it in a given location. As stated above, we'll need the associated configuration file that contains your API Key, API Secret Key, API Token, and API Token Secret - mine is saved in a file called config.py. We'll also need the pathway to your python.exe file (mine was "C:\Users\erich\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe").
