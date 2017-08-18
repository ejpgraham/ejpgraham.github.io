---
layout: post
title: Bloc Jams
thumbnail-path: "img/blocjams/blocjams.png"
short-description: Bloc Jams is a music playing app modeled on Spotify.

---

{:.center}
![]({{ site.baseurl }}/img/blocjams/blocjams.png)


## Meet AngularJS

During the Bloc curriculum, Bloc instructs its students to create a music player app as their first complex project. First, following the Bloc guidelines closely, I created a working music app using mainly plain javascript and jQuery. The resulting app contained enormous, unwieldy blocks of code that proved difficult to navigate. The main javascript file, for instance, was over 300 lines of code, making it difficult to update and change the app as I worked. The html for components that appeared on multiple pages such as the player bar was repeated on each page, violating the DRY rule.

<img src="/img/blocjams/blocjamslayout.png" align="left" width="150" height="425" />

During the javascript bloc-player instruction, Bloc does not hint that using plain javascript without a framework to create a dynamic one-page app is not really a good idea! Converting the project to AngularJS is a breath of fresh, organized air, and the Bloc-Player-Angular project does a good job of exploring why it’s worth using Angular in the first place. Frameworks require a lot of precise and fairly complicated syntax and they do not strike me as simple to learn.


Over the course of the Bloc-Player-Angular project, I broke up the enormous blocks of code into multiple files, instead storing the logic in discrete, organized services, filters, and directives.  Controllers now supply the various templates of the site with information such as the currentAlbum and whether a song is playing or not.


## Learning Angular

I’ve worked a little with React, so the overall concept of a single page app and the framework or library needed to make it run are somewhat familiar with me. I found it a bit hard at first to understand the syntax for setting up an AngularJS component. I was pleased to learn that controllers, services (and the many types of services, including the… service service), directives, and filters are structured the same way. Here’s one of my controllers as an example:

{% highlight javascript %}
  (function(){
    function AlbumCtrl(Fixtures, SongPlayer) {
      this.albumData = Fixtures.getAlbum();
      this.songPlayer = SongPlayer;
    }

    angular
      .module('blocJams')
      .controller('AlbumCtrl', ['Fixtures', 'SongPlayer', AlbumCtrl]);
  })();
{% endhighlight %}

This setup caused some confusion for me because not all of the documentation used the same formatting. This was a valuable lesson to learn early on, as I found I could still use documentation to answer my questions even if it was a different style than what I was using. The [angular site](https://docs.angularjs.org/guide/controller) sets up a basic controller like this, which looks different from how Bloc suggested I do it (which in turn is from [Opinionated Angular]https://toddmotto.com/opinionated-angular-js-styleguide-for-teams/):

{% highlight javascript %}

var myApp = angular.module('myApp',[]);

myApp.controller('GreetingController', ['$scope', function($scope) {
  $scope.greeting = 'Hola!';
}]);
{% endhighlight %}

Gradually, every part of the setup made sense. The entire controller or service is wrapped in an anonymous, self invoking function to preserve the global namespace. Components can be “injected” into other components in order to give them access to the useful code I’m writing. Directives are used to update the visual interface, services contain the “business logic” (the code needed to make the app function), and filters standardize and format the information displayed in the app.


## The results:

After breaking down the app into Angular components, my code is much more organized. I know that if there’s a bug affecting the “Next Song” button, the code in question is in the SongPlayer service, as that’s responsible for tracking the songs in the album. This setup also gives a roadmap for future features: A login feature would originate in the Landing Controller and require a cookie service. Favoriting a song or adding it to a playlist could be featured in the Player Bar Controller, and so forth.

I also found that as the app grew in complexity, it became increasingly difficult to locate bugs without a debugger. There are too many variables and functions to keep track of. Thankfully Chrome’s debugger tools are free and extremely useful. It’s times like this that make me thankful I’m learning to code now and not say, 10 years ago before Chrome was released.

Google also proved to be my best friend during development. Not only was it instrumental in finding the correct documentation, I was often surprised at how many of my bugs were extraordinarily well-explored. It’s a reminder that as I learn, I’m quite unlikely to face a problem that’s never been faced before, and the many coders that came before me are here to help!

## TLDR:

1. AngularJS, or any framework, is an upfront investment for a payoff of modular programming for a dynamic single page app.
2. Documentation can seem intimidating at first, but is vital to expanding a coder’s knowledge and abilities.
3. When you encounter a bug, debugger and Google are your best friends.
