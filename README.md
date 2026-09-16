Vino
====

Vino is a trls.uk API client written in JavaScript. It is targeted toward
Node.js, but should be usable via the browser, CORS issues notwithstanding.

~~This library was not authorized by the good folks over at Vine HQ. I built it
by using the excellent Fiddler2 network monitoring software to watch what the
official iOS client did while running on my iPad. It may not work at all for
you, or may contain terrible bugs, or may piss off Vine.~~ Guys guys...the api has documentations and i just ate Vine,so no need to worry...

Features
--------

- Can log in to your account (but not via Twitter because the platform is shit ~~and you shouldnt use it~~); you need to have set an
	actual email and password)
- Can fetch your home feed
- Can find posts by tag
- That's about it. 

Example usage
-------------
Assuming you already have Node and NPM installed...
```shell
git clone git@github.com:tlack/vino.git
cd vino
npm install
node
```
Then...
```javascript
var vino = require('./vino');

client = new vino({username: 'barf@symptoms.com', password: 'barfsymptom'});
client.login(function(err, key, username) {
	
	if (err) throw new Error(err);
	console.log('successfully logged in, key and username: ', key, username);

	client.homeFeed(function(err, feed) {
		if (err) throw new Error(err);
		// console.log('your timeline', feed);
		for (var i in feed.records) {
			var entry = feed.records[i];
			console.log(entry.description, entry.videoUrl);
		}
	});

});
```

In the pipeline
---------------

~~- Research workaround for Twitter OAuth~~ Never happening.
- Get other users feeds
- Improve request() boilerplate disaster

Random Vine API notes
---------------------

Here's some random stuff I've sussed out with Fiddler2.

- The protocol is delightfully simple. It's just HTTP over SSL using standard verbs, etc.
	This was a walk in the park compared to Snaphax.

~~- Every few minutes the client requests http://vines.s3.amazonaws.com/static/status.json
  with some headers to check cache expiration. What is this URL used for? To alert users
	when the service is down?~~ dont think thats used.. current day it throws a Access Denied XML error..

~~- (Updated) The user profile endpoints (/users/profiles/[SuperLongUserIdInt]) returns the
	users phone number. I find that strange. Perhaps it's useful for finding
	people to follow using your contacts, but even so I feel that functionality
	should be server side. Update: This no longer occurs. The phoneNumber field still
	appears for some users, but its value is null.~~~ Trellis doesnt use phoneNumber..

~~- There's a "secret" web interface for the Explore pane that can be accessed
	without login via your web browser: http://vine.co/explore . I'm a bit
	puzzled about what this is needed for or why they implemented the system this
	way. Alas, all the links are using proprietary in-app URIs~~ Trellis uses that too

License
-------

(C) Copyright 2013 Thomas Lackner <lackner@gmail.com> 
MIT license

Disclaimer
----------

I am not employed by Trellis, nor have they blessed this project (But me,as a fox,blessed this with Trellis). 

Warranty
--------

Absolutely none
