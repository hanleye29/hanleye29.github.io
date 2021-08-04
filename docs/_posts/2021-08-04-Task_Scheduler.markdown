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

# Overview

What is Windows Task Scheduler? Windows Task Scheduler is a tool that allows you to create and run virtually any task automatically. Rather than sitting there pressing execute over and over and over in Jupyter Notebook or your IDE, Windows Task Scheduler is able to run these tasks for you at prespecified times. Actually, if you open Windows Task Scheduler right now, you'll see applications that have already set up a number of automated tasks. 


<center><img src="https://github.com/hanleye29/hanleye29.github.io/blob/main/docs/_includes/scheduler_overview.PNG?raw=true" style="height: 300px; width:600px;"/></center>


Generally, a task consists of a trigger (which can be time or event based) and the action to be carried out. The action is typically the execution of a .exe (executable) file or a .bat (batch) file. In our case, our trigger will be time based - it will occur every 5 minutes - and the action will be the execution of a batch file. Let's take a look at how we'll put together this batch file.

# Batch File

A batch file or batch job is a collection, or list, of commands that are processed in sequence often without requiring user input or intervention. With a computer running a Microsoft operating system such as Windows, a batch file is stored as a file with a .bat file extension. A batch job can accomplish multiple tasks without interaction from the user, freeing up the user's time for other tasks. Our batch file will consist of three commands:

1. A command to start python.exe.
2. Once we're running python, we'll want to execute our pull_ten_tweets.py
3. Pause the execution of the program so we can so our command prompt after running our first two commands.

That's really all there is to this.

## 1. Get Path to python.exe
We run python simply by providing the pathway to the the python.exe file on our system (mine was "C:\Users\erich\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe"). Just as a note, make sure you have tweepy and pandas installed if you want to run the tutorial exactly. To do this, open the command prompt and enter the following two commands:

```
python -m pip install pandas
python -m pip install tweepy
```

Terrific.

## 2. Set Up config.py & pull_ten_tweets.py

We'll need the associated configuration file that contains your API Key, API Secret Key, API Token, and API Token Secret. Not going to go over how to do that here, but there are plenty of great tutorials on that in other places. My keys are saved in a file called config.py as shown here (but not the actual codes, because then you could post all kinds of silly tweets to my account):

```python
#config.py

API_Key = 'XXXXXXXXX'
API_Secret_Key = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'
Access_Token = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXx
Access_Token_Secret = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'

```

Addtionally, we won't go into detail on the mechanics of the following code pull_ten_tweets.py. To summarize super briefly, this code will basically save the **desired_fields** and **user_fields** for the last **max_tweets** about **query** as individual .csv files in the location **reports_path**. So each time this code runs, all ten tweets will be saved as individual .csv files at a specified location. Additionally, our naming convention for the .csv files prevents duplicates from being saved - if the same tweet is read in two runs, it will simply overwrite the first saved instance.  I chose bitcoin for my query because I'm intolerable.

```python
#pull_ten_tweets.py
import config
import pandas as pd
import tweepy as tw

#Set up API connection with keys
auth = tw.OAuthHandler(config.API_Key, config.API_Secret_Key)
auth.set_access_token(config.Access_Token, config.Access_Token_Secret)
api = tw.API(auth, wait_on_rate_limit=True)

#Set tweet topic, number of tweets to return
query = 'bitcoin'
max_tweets = 10

#Search returns a list of JSON files
search =[status for status in tw.Cursor(api.search, q = query).items(max_tweets)]

tweet_data = []
desired_fields = ['created_at', 'text','lang']
user_fields = ['name','screen_name','location','description', 'followers_count','friends_count']
temp_dict = {}

for i in range(len(search)):
    temp_dict = {}
    for key, value in search[i]._json.items():
        if key in desired_fields:
            temp_dict[key] = value
        elif key == 'user':
            for key2, value2 in search[i]._json['user'].items():
                if key2 in user_fields:
                    temp_dict[key2] = value2
    tweet_data.append(temp_dict)

labels = list(tweet_data[0].keys())
df = pd.DataFrame(columns = labels)
for i in range(len(tweet_data)):
    df = df.append({
              labels[0]:tweet_data[i]['created_at'],
              labels[1]:tweet_data[i]['text'],
              labels[2]:tweet_data[i]['name'],
              labels[3]:tweet_data[i]['screen_name'],
              labels[4]:tweet_data[i]['location'],
              labels[5]:tweet_data[i]['description'],
              labels[6]:tweet_data[i]['followers_count'],
              labels[7]:tweet_data[i]['friends_count'],
              labels[8]:tweet_data[i]['lang']}, ignore_index=True)

for i in range(len(df)):
    name = df['screen_name'][i]
    time = df['created_at'][i].replace(" ", "").replace(":","_").replace('+',"_")
    df.iloc[i].to_csv(reports_path + '\\' + query + "_" + name + "_"+ time +'.csv')

```
Got that? Great. Save all that to a folder and toss another "reports" folder in there to which you can save your .csv files - make sure to provide that path to the last line in pull_ten_tweets.py. Now you should have a folder that looks like the following (excepting pycache and the batch file):

<center><img src="https://github.com/hanleye29/hanleye29.github.io/blob/main/docs/_includes/scheduler_folder.PNG?raw=true" style="height: 300px; width:600px;"/></center>

Now for the extremely simple part of making the batch file. Open your notepad and plop in the path to your python.exe, press the spacebar, and then plop in the path to your pull_ten_tweets.py, press enter and then type in "pause". Should look like this:

<center><img src="https://github.com/hanleye29/hanleye29.github.io/blob/main/docs/_includes/batch_pic.PNG?raw=true" style="height: 300px; width:600px;"/></center>

Save that bad boy as real_batch.bat in the same folder as your .py files, and you're ready to ball. You can actually double click this batch file and now it should execute. I would advise you do this to make sure everything is running properly - otherwise you'll crash during task execution.

# Schedule the Task
Great news - this is the easy part. Open Task Scheduler, right click the Task Scheduler Library, and create a New Folder called "MyTasks". Right click MyTasks then select "Create Basic Task". In the wizard, provide a name of your choosing and a description. For Trigger, select "One Time" and schedule the event for, say, 15 minutes from now. For Action, select "Start a Program", then browse to your batch file "real_batch.bat". On the Finish screen, check the box that says "Open the Properties dialogue for this task when I click Finish" and then select Finish.

Holy cow you're so close. Now the Properties dialogue has appeared. Select Trigger, and then Edit the trigger. Under Advanced Settings, set *Repeat task every:* to **5 minutes**, and set the duration to **Indefinitely**.

<center><img src="https://github.com/hanleye29/hanleye29.github.io/blob/main/docs/_includes/edit_trigger.PNG?raw=true" style="height: 300px; width:600px;"/></center>

Last thing! Open conditions and uncheck the box "Start the task only if the computer is on AC power" - otherwise, you won't be able to run this task unless you're plugged into a power source. This one tripped me up for a moment when I was banished from the living room and AC adapetr while my wife was on a call.

<center><img src="https://github.com/hanleye29/hanleye29.github.io/blob/main/docs/_includes/edit_conditions.PNG?raw=true" style="height: 300px; width:600px;"/></center>

### Now We Wait!
Did it work? How can you even tell? Well, as soon as you hit that first run time instance, you should see a window pop up on your screen that looks like the following:

<center><img src="https://github.com/hanleye29/hanleye29.github.io/blob/main/docs/_includes/success_popup.PNG?raw=true" style="height: 300px; width:600px;"/></center>

This pop up is happening because of that "pause" statement we have at the end of the .bat file. We need to provide an input to let the .bat file continue, and if we do, then it ends (because that's the last command). If we erase that statement, then the task will execute silently in the background, and you won't have to worry about pop ups. Which are annoying!

Additionally, if you navigate to where you're storing your .csv's you should see something like this:

<center><img src="https://github.com/hanleye29/hanleye29.github.io/blob/main/docs/_includes/csv_success.PNG?raw=true" style="height: 300px; width:600px;"/></center>

Finally, five minutes after the first instance runs, you should get another pop up to let you know that your task is running - you should also have ten more .csv files saved in there.

# Great News
Really terrific work out there. Now you can bump up those requests and start doing some real, hardcore twitter analytics. Of course, it would likely make more sense to have this type of thing set up on a cloud computing service so you can feed these into an analytics service. You would use a cloud scheduling service like GCP Cloud Scheduler or AWS Eventbridge to run a Cloud Function or a Lambda Function respectively, but this is basically the same process as we've seen here - just done in the cloud. Doing these things in the cloud makes the overall flow of data a lot easier to manage - you can pipe these tweets directly to Pub/Sub services or Data Warehouses for analysis. From a scalability standpoint, you probably won't be able to predict bitcoin's price tomorrow based off ten tweets, seven of which are from a bot named musky_coin_boy_6900. Once you start collecting BIG DATA - you'll need more than you laptop.

Still a fun exercise regardless. Follow me on LinkedIn I guess. Not Twitter though - that's for personal use only and it's primarily Sopranos themed anyways.
