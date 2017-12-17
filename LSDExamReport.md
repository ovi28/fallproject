# Introduction
Originally, we started doing our Hacker News Clone the way it was taught in class, but after some group changes we decided to change the way we did it and we focused on using Amazon Web Services.
# 1. Requirements, architecture, design, and process
## 1.1 System requirements
The purpose of the system is to be a functional social news website focused on computer science. The HNClone(Hacker news clone) is a website where registered users are able to post stories and non-registered users can only see these stories, but cannot contribute in any way. The website is inspired by the already existing Hackernews.
The functional requirements are as follows:
*    The user must be able to view news on the main page
*    The user should be able to login
*    The user should create a new account
*    The user should add news
*    The user should be able to comment on news and other comments
*    The user should be able to vote if he has the necessary karma
    
The nonfunctional requirements are as follows: 
*    System must inform the user whenever an error is encountered
*    System must show a loading indicator whenever the frontend is awaiting an answer from the backend
*    The software will be packaged using containers
*    System updates should not result in data loss
*    The system should run on any modern browser (Chrome, Firefox, Edge, Safari, etc.)
*    All graphical user interfaces(GUI) will be rendered in a web browser
## 1.2. Development process
We were only 2 people in the project originally and we split the roles as follows: 
Ovidiu: product owner, lead developer, DevOps engineer
Mohamed: architect, quality assurance/testing lead
After Rumyana joined our project, among other tasks, we gave her the DevOps engineer role.
We used the following development processes: code review, poker planning, paired programming.
We also used Scrum and we divided the sprints into 2 weeks initially, but we adapted based on when we actually finished each sprint.
## 1.3. Software architecture
Originally, we planned on using Docker containers for our project, but we later switched to the idea of using an Amazon ElasticBeanstalk PHP server which would host both the backend and the frontend and host the MySQL database using Amazon Relational Database Service(RDS).
AWS has a few zones where they have servers all over the globe and we picked Europe zone 1(Frankfurt) to host both the Database and the ElasticBeanstalk server. This server is the closest to our target audience.
Using PDO, a PHP library, we connect and interact with the MySQL database.
Even though the frontend and backend are hosted on the same server, they communicate through REST calls. This would allow them to be on separate servers in the future.
For the frontend’s CSS we are using the original CSS from Hackernews.
## 1.4. Software design
Our biggest concern was being able to provide all the required functionality described in the course because we chose a different path for our project from the recommended one by not using Docker, Jenkins and using PHP as our main development language. We also didn’t have any prior experience with AWS.
Instead of using Jenkins with Docker containers we used AWS’ Code Pipeline. Our project was hosted on CodeCommit which is a GitHub alternative made by Amazon with the bonus of being private. That way we could store our passwords in the repository.
The ways Code Pipeline works are:
*    Someone pushes something to the repository
*    Code Pipeline detects the change
*    It uploads the changes to the ElasticBeanstalk server with 0 downtime
![Pipeline](https://raw.githubusercontent.com/ovi28/fallproject/master/ss/pipeline.png "Pipeline")
Another problem that we faced was outputting the proper status of the /status endpoint. We managed to have the correct status shown by using AWS’ provided Php API to connect to Code Pipeline and to ElasticBeanstalk’s health status.
If the Code Pipeline status was reporting that something is updating, the status would change to update.
ElasticBeanstalk has a few possible statuses, the one that interested us was “Ok”. If the status was anything else, we would show that the website was down.
![Status](https://raw.githubusercontent.com/ovi28/fallproject/master/ss/status.png "Status")
We discovered another problem when we published the website, as every page was accessible by using .php at the end and the simulator required the pages to be accessible without the .php extension. 
We solved this by creating a .htaccess file. The .httaccess file, when placed in the root folder, will be loaded by the server and it can server settings. In our case, we made the .htaccess file redirect the 404 pages to their equivalent with .php at the end. /status becomes /status.php and so on.
![hta](https://raw.githubusercontent.com/ovi28/fallproject/master/ss/htaccess.png "hta")
## 1.5. Software implementation
Choosing Php proved to be a good decision. As an interpreted language, it doesn’t need building before deployment, which simplified our CD/CI chain.
We created a different endpoint for each main website functionality on the backend side: register, login, getting articles and posting articles.
The main page requests the last 50 posts from the getArticles page and displays them. By clicking more, the next 50 are loaded. The posts are passed between using JSON.
We made a special /post.php page for the simulator and we modified the database to include a hannestid field. We still use another field as the primary key.
The following requirements were not met in the final version of the website:
*    We didn’t implement voting and comments on the front end and backend. The database supports them and has the necessary tables.
*    We are not using containers. ElasticBeanstalk has all the future that we need, including a feature called Elastic Load Balancer. Elastic Load Balancing distributes incoming application traffic across multiple EC2 instances, in multiple Availability Zones. 
*    We are only showing errors when the user is creating an account, logs in or creates a post and there is a problem.
AWS provides logging and monitoring with alarms after setting them up. Logs are showing all incoming connections and they provide a special section for errors.

We set up monitoring for the Health status of the environment, environment latency, and database latency.
# 2. Maintenance and SLA status
The person operating the other group is Rumyana. She, in the role of an operator, was responsible for ensuring the system of other group was running properly in the period 12/04/2017 – 12/15/2017.
## 2.1. Hand-over
The other group handed over to us a link to their Github repository, containing their source code and a “README.md” file, which presumably contained the technical documentation for their system. This file describes the 3 conceptual parts of their system, how to access their virtual system environment (droplet), how to get permission to operate the system, a description of their API routes, how to report an issue and a link to their monitoring system (Grafana dashboard).  
Let’s discuss the content of the documentation. The system overview was too short as it contains only one sentence: “This is a clone application of the website HackerNews”. [1] 
I think it should also explain the functionalities of the system. Next, a brief description of the system architecture describes the three running components of the system and their web addresses. I was also informed of how to gain access to the system administration and the database, and how to start and stop the application. This gives me the ability to control the application from within. I couldn’t find a description of the data flow within the system, but from the system architecture, I figured that the back-end manipulates the data from the database and presumably contain the API. Unfortunately, the website is not described at all, neither are the web pages or the functionality of it. This is not acceptable because font-end is one of the three main components of the system. The API, on the other hand, is described well, the only things missing are the format and structure of data that the “/post”, used to post a new post, and “/user” to post a new user. During the maintenance process, we discovered that the routes are wrong and some of them were missing. Finally, it says to “contact one of the team members” [1] in case of something happening, but it’s not described how to contact them. I figured one way to do so is post a Github Issue [2] on their repository and another to talk to them in person.  
In conclusion, the documentation contained enough information to understand how to operate two of the systems and react to errors. It was lacking a description of one of the system components and some more information on one of the other components. I partially prepared to maintain the system. 
## 2.2. Service-level agreement
After that, we set up a service-level agreement. This agreement looks like this: 
“This Service-level Agreement (SLA) will define the contract that we, the service provider, will try to guarantee at all times to you, the client. We describe an agreement that we have set in order to have a service that meets all expectations. We set the expected up time of the service, response times and status as well as explain how we calculate the metrics.
Uptime
Our services minimum expected up time is set at 95% per month. This means that the website Hacker News will be at a minimum 95% of the month. The way in which we calculate the percentage of up-time of the service is by taking the number of minutes in the month and comparing the minutes of up-time we have had that month.
Response time
Our service will have a maximum response time of 100 milliseconds. This metric is calculated with the help of an external service, Prometheus. Prometheus will store the response time of all requests sent to our server and the time it takes the server to respond to the client, from this we will be able to see if any request surpasses a response time of 60 milliseconds. This, of course, is subjective depending on whether the client internet connection is strong. If the client internet connection is by any means deemed to be not of adequate speed this agreement can be acquitted.”
From the documentation, we received it’s clear we can monitor the two metrics we agreed upon. They can be accessed from the link to their Grafana dashboard.
## 2.3. Maintenance and reliability
I, Rumyana as a system operator was responsible for the maintenance process. First I contacted the other group members to address the communication issue. Together we verbally agreed to address any issues on their Github repository as I presumed. Additionally, we can contact them via their profiles on Facebook. Additionally, they described the website functionalities. 
Second I tested all of the system functionalities. Starting with trying the website functionalities by visiting the website. I was able to view a list of posts on their front page, register and login via a separate page, upload a post as a user. All of the pages contain a green navigation bar on top witch makes it easy to browse through the website. In general, everything seems in place. 
I continued to test the API. After following the description provided in the documentation it became clear that some of the URLs were not correct. I contacted the team via their Github repository using Github Issues [2] by reporting two of the mistaken URLs: the post a new post and post a new user. Additionally, I mentioned the lack of description of the format of the POST body for posting a new story. After just two hours the group has fixed the issue by specifying the format and updating the README with the correct URLs.
For testing the API I used Advanced REST Client [3] chrome extension. I began by posting a couple of posts as JSON files in the specified format. These posts examples were taken from a tester program made specifically to test the system’s API [4]. On some of the POSTs the system responded with a code 200 but with a message body containing hat seemed to be a SQL error message. 
![alt text](https://i.imgur.com/6qKaPhV.png) 
I contacted the group via Github by posting a Github Issue. The reported issue was fixed by changing the SQL table’s row type “from using varchar(45) to varchar(200)” [5]. After consulting with the mandatory requirements [6] I found that “http://146.185.141.49:8081/latest” was not recorded in the technical documentation they provided, this issue was not reported. I continued with checking the “latest digested post” by submitting a post with a “hanesst_id:42” I was able to retrieve the latest digested post and confirmed it was the same. The system also provided it’s status when requested on “/status”.
Finally, I checked the metrics specified in the SLA. I did so by comparing the expected metrics from the SLA and the recorded metrics from Grafana dashboard.
The defined response time should be a maximum of 100 milliseconds. The recorded metrics show that for the majority of requests the response time has indeed stayed under 100 milliseconds with a few exceptions. On the 07/12 and 12/12, the system exceeds the limit, but as stated in the SLA “This, of course, is subjective depending on whether the client internet connection is strong.” [7] This is the reason these two issues were not reported.
The system uptime agreement was completely upheld as the recorded uptime is 99.28237192825749% and the expected was 95%. This metrics is recorded by running this query on their Grafana dashboard “avg_over_time(up{job='backend'}[30d]) * 100”.
In conclusion, I can say that the developers responded to all reported issues. There was a slight delay in responding to the problem with “/post” route, but since there was no stated respond time in the SLA we cannot hold them responsible. The system has an excellent uptime and good response time with two minor fluctuations. I think the system is reliable in general since I consider the SLA agreement has been kept and all of the issues addressed. 
# 3. Discussion
## 3.1. Technical discussion
The development process was a bit chaotic, as it usually happens, which was mostly caused by choosing a different way to create and run our HackerNews clone. It proved advantageous at the same time as some things proved to be easier to implement, like the CD/CI chain and everything is better integrated as all the services are provided by Amazon.
The maintenance process went smoothly considering the lack of some information in the initial hand-in. It would have been much better if the developers had put more effort in describing the system’s functionality. This would have given me more guidance when testing the system and also trace the issues I encountered. It’s also very important to ensure the feedback channels are clearly stated. I made a lot of presumptions about that. I consider as my mistake not requesting to rewrite the service level agreement when I first encountered this error and the consequences were shown later when the developers were late in responding to some of the issues. 
## 3.2. Group work reflection & Lessons learned
Our most important lesson was that we should create documentation while doing the project, not after. As most of our project's documentation was created after, the process was more difficult.
We were pleased with how easy it is to work with AWS and how well integrated the services are(CodeCommit, CodePipeline, RDS, ElasticBeanstalk, and CloudWatch) and if we would have to do this project again we would use them again.
Even though we assigned roles to each other, we failed to stick to them and at times we switched roles. In the future, we will try to stick to them.
# References
1 https://github.com/MDJ-Mikkel-Djurhuus/hackernews-clone/blob/master/README.md
2 https://help.github.com/articles/about-issues 
3 https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo 
4 https://github.com/UsagiBo/soft2017fall-lsd-teaching-material/blob/master/assignments/student_tester.py
5 https://github.com/MDJ-Mikkel-Djurhuus/hackernews-clone/issues/3 
6 https://github.com/datsoftlyngby/soft2017fall-lsd-teaching-material/blob/master/assignments/03-Minimum_Requirements_and_API_Description.md
7 http://146.185.141.49:3000/dashboard/db/hackernews?orgId=1&from=1512082800000&to=1514761199999&refresh=5s&panelId=6&fullscreen 

