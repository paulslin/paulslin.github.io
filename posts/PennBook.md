# PennBook
---
**Author**: Paul Lin, Katie Yang, Henry Enrique Sherwood Cabello, Caroline Leung

**Date**: December 2020

**Project Description:** 
PennBook is a Facebook-like application with 5 main functionalities: users and accounts, posts and comments, friends, chat, and newsfeed. Altogether, these five functionalities combined provide users a dynamic social network for browsing through content pertinent to their interests as well as staying connected with other users. The application database is hosted on Amazon's DynamoDB and the server on Amazon's EC2 services. Backend operations and processes are written primarily in Java and JavaScript and the front-end display is written in standard HTML/CSS/JavaScript format along with jQuery and AJAX.

## Users and Accounts
---
Within users and accounts, users can create account profiles visible to the PennBook community. During user registration, users will be prompted to input a series of basic information (username, password, birthday, email, etc.) and two special groupings: interests and affiliations. The interests grouping includes media categories such as Politics, Tech, and Travel and is used for newsfeed recommendation. The affiliations grouping includes demographic and hobby groupings such as New York, University of Pennsylvania, and Soccer and is used for identifying similar users. After creating their account, users can continue changing and updating their basic information, as well as their interest and affiliations.
<p float="left" align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/PennBook/login.PNG?raw=true" width="49%">
<img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/PennBook/interest.PNG?raw=true" width="49%"></p>

## Posts and Comments
---
Within posts and comments, users can dynamically create posts on their own wall or another user's walls, post comments under them, share posts made by other users, and like and dislike posts. Since post activities are loaded via AJAX requests, any post activity performed on one user's end will be updated real-time on another user's end. After a user logs in, their profile page will be filled with relevant posts related to theirs and their friends' social activities. Moreover, a reddit-like restrictive like/dislike system is implemented, meaning that users can either like or dislike, but not both simultaneously. Much like Reddit, should a post receive an overwhelmingly large proportion of dislikes from multiple users, the post’s content is hidden from all users. This could be useful for filtering content deemed undesirable by the user community (e.g.: fake news, threats of violence, etc.). Each user can only cast one vote, thereby ensuring that a single user cannot singlehandedly hide a content by casting multiple dislikes.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/PennBook/profile.PNG?raw=true"></p>

## Friends
---
The Friends page is based off of the current Facebook friends page. Users can view and remove their current and active friends, discover new users to friend, and discover the interesting connections between their friends and affiliations. When discovering new friends, users will be recommended friends with similar friends, interests, and affiliations. Finally, a friends visualizer interface allows users to visualize the social network graph within their community.
<p float="left" align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/PennBook/friends.PNG?raw=true" width="49%">
<img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/PennBook/visualizer.PNG?raw=true" width="49%"></p>

## Chat
---
The Chat page is built on socket.io and modeled after the Facebook Messenger system. On the left side, users can see the lists of chats, which will dynamically update if a new message is received/unread. On the right-hand side, users can select chats to send and receive messages within them. Both 1-1 and group chats are supported by the chat page, as well as bolding unread messages, editing chat names, adding new users, and leaving group chats. If users are browsing on other pages, messages from friends will result in real-time notifications appearing.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/PennBook/chat.PNG?raw=true"></p>
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/PennBook/chat2.png?raw=true"></p>

## Newsfeed
---
The newsfeed page is region that displays both unread recommended articles and friends' social activities. Article recommendations are based on two factors: 1) the user's interests and 2) a backend Adsorption algorithm that runs a PageRank-like analysis for prioritizing the most popular articles. Finally, a simple search bar is also included for allowing users to look up specific topics and articles.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/PennBook/newsfeed.PNG?raw=true"></p>


