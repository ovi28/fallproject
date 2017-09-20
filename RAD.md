## 1. Introduction
### A. Purpose of the system
The purpose of the system is to be a functional social news website focused on computer science.
### B. Scope of the system
The HNC(Hacker news copy) is a website where registered users are able to post stories, upvote or downvote other stories and also comment on them and other people's comments.
Non registered users can only see these stories, but cannot contribute in any way.
### C. Objectives and success criteria of the project
The success of this project depends on the following criteria:
* Having an uptime of more than 95%
* The system should not lose any content, not even when it's down for various reasons
### D. Definitions, acronyms, and abbreviations

### E. References
[Hackernews](https://news.ycombinator.com/)
[Reddit](https://www.reddit.com/)
## 2. Current system 
The development of HNC will be inspired by the already existing Hackernews website.
## 3. Proposed system
### A. Overview
This section provides the functional overview of the entire system.
### B. Functional requirements
* The user must be able to view news on the main page
* The user should be able to login
* The user should create a new account
* The user should add news
* The user should be able to comment on news and other comments
* The user should be able to vote if he has the necessary karma
### C. Nonfunctional requirements
#### a. Usability
* System must show a loading indicator whenever the frontend is awaiting an answer from the backend
* System must inform the user whenever an error is encountered
* Every interaction that the user has with the system must give feedback
#### b. Reliability
* A system crash should not result in data loss
* When the system is down, the data that it receives should be kept and used when the system is up again
#### c. Performance
* Animations must be smooth without lagging
* Actions (upvoting, posting stories etc.) will take little time to be completed, almost unnoticeable
* Any page of the website should render in under 5 seconds with an internet speed that is average (16.1 mbps at the time of writing)
#### d. Supportability
* The system should run on any modern browser(Chrome, Firefox, Edge, Safari, etc.). The oldest supported version is from January 2017
#### e. Implementation
There are no limitations regarding implementation
#### f. Interface
All graphical user interfaces(GUI) will be  rendered in a web browser
#### g. Packaging
The software will be packaged using containers
### D. System models
#### a. Scenarios
Name: vote
Actors: registered user
Flow: 
* actor opens the webpage
* actor logs in using username and password or creates an account
* actor picks a story and upvotes it/downvotes it

Name: comment
Actors: registered user
Flow: 
* actor opens the webpage
* actor logs in using username and password or creates an account
* actor picks a story or another comment, types a comment and presses the comment button

Name: login
Actors: user
Flow: 
* user opens website
* user goes to the login page and types in his credentials and presses the login button
* if the username and password are correct, the user will be logged in otherwise, he will have to type them again

Name: create account
Actors: user
Flow:
* user opens website
* user goes to the create account page and types in the details
* if the details are correct, the user will have a new account

Name: posting story
Actors: user
Flow:
* user opens website
* user user logs in if he is not yet logged in
* user presses the add story button, he types in the details of the story and submits it

Name: test from API
Actors: testing system
Flow: the actor tests the functionality of the website by accessing the API: voting, creating accounts, using the accounts, commenting, posting stories
#### b. Use case model
Name: comment
Actors: registered user
Flow: 
* actor opens the webpage
* actor picks a story or another comment, types a comment and presses the comment button
Entry condition: The user is logged in
Exit conditions: The comment appears on the website
Quality req: The comment appears instantly on the website

Name: vote
Actors: registered user
Flow: 
* actor opens the webpage
* actor votes on a story
Entry condition: The user is logged in
Exit condition: The vote is registered
Quality req: The vote appears on the website

Name: login
Actors: user
Flow: 
* user goes to the login page and types in his credentials and presses the login button
* if the username and password are correct, the user will be logged in otherwise, he will have to type them again
Entry condition: User wants to log in
Exit condition: User is logged in
Quality req: The user must exist before the login process is started

Name: create account
Actors: user
Flow:
* user opens website
* user goes to the create account page and types in the details
* if the details are correct, the user will have a new account
Entry condition: User wants to create an account
Exit condition: The account is created 
Quality req: The new account id must be unique

Name: posting story
Actors: user
Flow:
* user opens website
* user presses the add story button, he types in the details of the story and submits it
Entry condition: user is logged in
Exit condition: The story is posted
Quality req: The story must appear instantly on the website

Name: test from API
Actors: testing system
Flow: the actor tests the functionality of the website by accessing the API: voting, creating accounts, using the accounts, commenting, posting stories
Entry condition: the actor gets the API key
Exit condition: the actor receives an answer from the API
Quality req: The API must respond in under 5 seconds
