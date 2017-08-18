---
layout: post
title: Old School Chat
thumbnail-path: "img/blocchat/blocchatlogin.png"
short-description: Build an online chat application

---

{:.center}
![]({{ site.baseurl }}/img/blocchat/blocchatlogin.png)

## Bloc Chat vs. Bloc Jams

Bloc Chat is the second major project in the Bloc curriculum. Unlike Bloc Jams, this project is built from scratch using AngularJS. So rather than converting from an existing Javascript project, I am now building an app with AngularJS in mind from the ground up. Bloc Chat is a smaller, leaner app than Bloc Jams, because I don't have to bother with the complicated logic of playing a song and managing the passage of time. Instead, we're mostly just displaying information.

## What's in a chat app?

The most unfamiliar part of Bloc Chat's instructional material is using a database to store information. A chat application, of course, needs to store and immediately display input from multiple chatters. Locally storing information like I did with the basic version of Bloc Jams is not an option.

Additionally, there's the issue of needing to log users in. The clear solution is to use cookies -  they are how all websites store information about users, including your login name and whether you're online.

Finally, the instructions for this project are bare bones and do little in the way of explaining how implement cookies and database access. It's clear Bloc is prodding me into figuring out how to do things on my own. This is an added challenge but a valuable opportunity to practice reading and using documentation for unfamiliar subjects.

## It's all in the database

In order to connect to a database, Bloc recommended [Google Firebase](https://firebase.google.com/). I was not familiar with this service, and it requires a some specific language to use correctly. It fit pretty easily in the Angular setup, and my Room Controller used it to pull information from a database and populate an array with it. Each message was an object containing the message's relevant information: the user who wrote it, its content, and so on. This is how I created my first messages, and displayed them in the app:

{% highlight javascript %}
(function() {
  function Room($firebaseArray) {
    var Room = {};
    var ref = firebase.database().ref().child("rooms");
    var rooms = $firebaseArray(ref);

    Room.all = rooms;

    return Room;
  }

  angular
    .module('blocChat')
    .factory('Room', ['$firebaseArray', Room]);
})();
{% endhighlight %}

 One problem I faced was how to associate certain messages with certain rooms. Thankfully, Google Firebase automatically creates a handy and unique key for every object it is instructed to create non-manually. In the Message Service, I created a function that fetched a room's messages:

{% highlight javascript %}
 Message.getByRoomId = function(roomId) {
      return $firebaseArray(ref.orderByChild("roomId").equalTo(roomId));
    };
{% endhighlight %}

This was made possible when the message is created - it is automatically assigned the active room's unique key to make the association between the two:

{% highlight javascript %}
var currentMessage = {
  roomId: $scope.activeRoom.$id,
  content: newMessage,
  username: currentUser,
  sentAt: new Date(),
};
{% endhighlight %}

Bloc's instructional material had me create the initial rooms and messages manually - a fairly tedious process. I was glad to have them created automatically, but seeing the structure of Firebase Array objects was vital to understanding the project:

<img src="/img/blocchat/blocchatdatabase.png" width="400" height="600" />

I just needed a way to log in users so they could actually post messages. This was actually a lot simpler than I anticipated using [angular-cookies.js](https://docs.angularjs.org/api/ngCookies). The browser stores the cookie, and is thus able to recognize if someone is logged in:

{% highlight javascript %}
(function(){
  function BlocChatCookies($cookies, $uibModal) {
    var currentUser = $cookies.get('blocChatCurrentUser');
    if (!currentUser || currentUser === ''){
      $uibModal.open({
        templateUrl: '/templates/login.html',
        size: 'sm',
        controller: 'ModalCtrl',
        backdrop: 'static',
        keyboard: false,
      });
    }
  }

  angular
  .module('blocChat')
  .run(['$cookies', '$uibModal', BlocChatCookies]);
})();
{% endhighlight %}



## The results:

Unlike Bloc Jams, the instructional material for Bloc Chat does not reference css or styling in general. At the end of the instructions I was left with a function, if boring application. I wanted to make this app mine in a way Bloc Chat was not due to its rigid styling. I see a lot of minimalist, sleekly styled websites these days and for fun I wanted to create a funny and unusual theme for my chat app.

I decided on a retro look which mimicked the old-school command command line, especially green text on a black background. I wanted blocky, pixelated fonts so I found a pair of perfect retro fonts on my good friend Google Fonts! I invented a good look for buttons (which don't exist on a command line, of course) and added some themed data to tie the look together. I am quite pleased with the result, which sacrifices a little clarity for a big dose of nostalgia:

{:.center}
<img src="/img/blocchat/blocchatlayout.png" width="1000" height="600" />

## TLDR:

1. Connecting services like Google Firebase to your app can be challenging at first, but is great for your app's features.
2. Cookies seem complicated, but aren't too hard to implement using existing tools.
3. A barebones project is a great opportunity to flex your creative muscle.
