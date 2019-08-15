---
title: So Many Frameworks, So Little Time
excerpt: At the end of the day, I realized there's nothing wrong with Rails. With 5.2 there's WebPacker and ActiveStorage which makes it a really solid platform. No, it's not as quick and responsive as a PWA ... but you can build a PWA driven and served by a Rails back-end. So you can use Rails for your account admin, content creation, and settings and what-not (where you need lots of forms that don't really demand high performance), and then use PWA's and Apps for your day-to-day functionality ... and Rails can be either the application server or the object broker, depending on what it's talking to. 
---

I spent a little time this week diving back into JavaScript frameworks. The funny thing is: Rails (for a dinosaur like me, anyway) is still the fastest way to go from "I got an idea" to deploying something on a server. Maybe because I'm used to it, maybe because it jives with how I work, maybe because it's built to be able to do just that. Whatever the reason, Rails will still be my Main Axe.

But some of these new technologies are cool and you can't **not** have at least a Mobile Web app anymore. So ***All Things Must Change***, as they say.

----

### Vuejs([https://vuejs.org/](https://vuejs.org/))

 ![]({{ site.url }}/assets/postimages/vuejs-logo.png "VueJS")

Up until now I used KnockoutJS ([http://knockoutjs.com/](http://knockoutjs.com/)) for my "active" JavaScript engine for Rails. When I need to load data over JSON and do fancy things with it, Knockout was effective at delivering that functionality without adding a ton of dependencies or mucking up the DOM. 

Vue is pretty nice at encapsulating components. In fact, you can encapsulate it (JS model, template, and CSS) into one `.vue` file so everything is nicely organized. Vue has a pretty wide ecosystem now and integrates very well with Rails 5's integration of WebPacker. You still kind of need to program "in two worlds" to use Vue, but at least it's all pretty well integrated with Rails so there shouldn't be too many hassles.

----

### ReactJS and Hyperloop
([http://ruby-hyperloop.org/](http://ruby-hyperloop.org/))

![]({{ site.url }}/assets/postimages/hyperloop-logo.png "Hyperloop")

I was really excited by Hyperloop. It essentially lets you code React code from Rails and lets WebPacker and the Asset Pipeline handle all the messy bits about putting things where they belong. Being able to code in "one world" would, for me, be a game-changer to using a JavaScript platform as an integral part of my stack.

Sadly, I couldn't get Hyperloop to work with Rails 5.2. And as I played around with React under WebPacker it just felt awkward to me. Seems like three quarters of the freelance jobs out there now require React, so I should probably know it better. But it feels too foreign to me now, and there seem to be a bunch of eccentricities that come as the price of entry. So ... nah.

----

### EmberJS
([https://www.emberjs.com/](https://www.emberjs.com/))

Ember is kind of the Rails of JavaScript frameworks. I tried it. When I saw the ginormous size of the nodejs code slug it downloaded I said: no thanks. Ember would make life too complicated for the way I like to develop.

----

### Ionic
([https://ionicframework.com/](https://ionicframework.com/))

![]({{ site.url }}/assets/postimages/ionic-logo.jpg "Ionic")

I stumbled across Ionic because something else I use also uses their icon set. But once I saw their docs, I really started wanting to try it. Basically it's a framework sitting atop a modified version of Angular. It incorporates a nice Rails-ish CLI, some good logical extension to Angular, and a very solid UX toolkit which works awesome on mobile devices. It also integrates Cordova so you can build mobile apps from the same codebase.

Ionic embraces Progressive Web Apps ('PWA') which is looking like the next wave to hit the web. One advantage to the PWA is it removes your reliance on App Stores as a delivery mechanism. Latest word is the new versions of Mobile Safari will support browser notifications, so the timing is right.

![]({{ site.url }}/assets/postimages/stencil-logo.png "Stencil")

The next major version of Ionic will include Stencil ([https://stenciljs.com/](https://stenciljs.com/)) which is a *'reusable web component compiler'*. This will make Ionic essentially platform agnostic - no longer totally reliant on Angular. This means you can build an "Ionic App" using Stencil and use Vuejs as your foundation library. Very cool.

![]({{ site.url }}/assets/postimages/capacitor-logo.jpg "Capacitor")

Also on the horizon is Capacitor ([https://capacitor.ionicframework.com/](https://capacitor.ionicframework.com/)) which will be an alternative to Cordova - it's the bridge between the JavaScript world and the native API's of the various build platforms.

The ability to build web PWA's and mobile apps, from one toolset, and be framework agnostic is a real game-changer.

----

### Hanami

[https://hanamirb.org/](https://hanamirb.org/)

![]({{ site.url }}/assets/postimages/hanami-logo.png "Hanami")

I tripped over this while reading new release notes from Heroku. Hanami isn't a JavaScript framework, it's kind of "Rails Lite". It does a lot of what Rails does, but without a lot of the bloat. It's smaller, and much faster, and takes a few different turns. For instance, you can host multiple apps from the same codebase - each with their own routing. This makes it an ideal platform for building micro-services. It doesn't have anywhere near the ecosystem of Rails, and it's unlikely to replace Rails, but I can see it having some very clear use cases where you want to code in Ruby with a Rails-esque code structure and you're willing to re-invent a couple of wheels (gems) in order to get super high performance.

----

### At the end of the day ...

![](https://media.giphy.com/media/3ornkczqjMrIUmdmSs/giphy.gif)

... I found myself more or less where I started. I like Rails. I can get things done. TurboLinks works fine for most "usual" cases - like Admin Consoles. Which you always end up needing. And for anything fancy that's user-facing, you can shut it off and there's jQuery and VueJS/KnockoutJS.

There's a whole new way of building things for the Web now that uses these frameworks and cloud-based everything (from authentication to storage to you name it). It's probably the Electrical Engineer in me that still likes everything more self-contained ... where the database and the code and everything is kind of "hosted together". But I do admire the flexibility of this new way of getting things done. And since most of the things I build tend to rely on complex relational data and/or complex back-end processing, Rails is still the best stack for my kind of work.

All that said, I do plan to try to build a proper mobile app/PWA for one of my commercial sites using Ionic - once Stencil and Capacitor are fully released. Without knowing Jack Squat about Angular I was actually able to prototype part of it in a couple evenings of tinkering and swearing. The UX toolkit is really nice too and folks are starting to play around with the tricks needed to use it for developing desktop/tablet format apps. 

Adding JWT to my Rails app wasn't that tough either and it only took another evening to get the prototype talking to the back-end. So for whatever reasons: I got on well with Ionic. Some of their "best practices" felt a bit arcane - like the layers needed between a Model and an API. But with more experience it will probably start making more sense.

And I do think I'll transition my active JS code to VueJS. I really wish there was tighter coupling to Rails, like what Hyperloop had promised. Or at least a "preferred way" to integrate Rails and VueJS. Maybe in time. 

And, at the end of the day, I realized there's nothing wrong with Rails. With 5.2 there's WebPacker and ActiveStorage which makes it a really solid platform. No, it's not as quick and responsive as a PWA ... but you can build a PWA driven and served by a Rails back-end. So you can use Rails for your account admin, content creation, and settings and what-not (where you need lots of forms that don't really demand high performance), and then use PWA's and Apps for your day-to-day functionality ... and Rails can be either the application server or the object broker, depending on what it's talking to. 
