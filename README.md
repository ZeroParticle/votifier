node-votifier
=============
**Emulates a Bukkit Votifier server in Node.js**

We at CubixCraft don't like Java that much. It kinda sucks a speed. So we always try to replace it with Node.js,
whenever it's possible. So is this case. Using node-votifier reduces load on your precious Bukkit and transfers
it to your wonderful Node.js app. Plus it's *super easy* to add vote listeners. No lame `.class` files anymore!

- - -

Requirements
------------
-   [Node.js](https://github.com/joyent/node/wiki/Installation)
-   OpenSSL ([`apt-cache search libssl | grep SSL`](https://help.ubuntu.com/community/OpenSSL/), pre-installed on Ubuntu servers)
-   RSA public and private key

I recommend using an Ubuntu server. However it should work on Windows / OS X / whatsoever, if your computer
supports the requirements listed above.

The RSA keys aren't automatically generated by node-votifier. But it's easy to create them yourself:

1.  `openssl genrsa -out private.pem 2048` generates a 2048 bit private key and stores it as private.pem
2.  `openssl rsa -in private.pem -pubout > public.pem` extracts the public key and stores it as public.pem

Use the public key for all the server lists. Keep the private key *private*. Otherwise anybody is able to
cheat on your vote system.

Installation
------------
**`npm install votifier`**

You could also clone this repository. If you wanna do this, you know how.

How to use
----------
Assuming your `private.pem` is in the same directory as your app, a working Votifier would look like this.
An extended example with error handling can be found in the `examples` directory.

```javascript
var votifier = require("node-votifier")(__dirname + "/private.pem");

votifier.on("vote", function(user, server, ip, date) {
  console.log(user + " voted.")
});
```

Complete API
------------
-   `require("node-votifier")` returns a `Votifier` Class.
-   `new Votifier(pathToPrivateKey, port)` or `Votifier(pathToPrivateKey, port)` both return a Votifier instance. They require the *absolute* path to the private key file. The port is optional and defaults to `8192`. Votifier inherits of [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter).
    - `vote` event. This event passes `user` (username of the voter), `server` (name of the server list), `ip` (IP of the voter) and `date` (date of the vote as a `Date`) to its listeners. It is fired, when Votifier receives a vote.
    - `error` event. This event passes `error` (`Error` with some infomation), to its listeners. This event is fired when something went wrong.

Contributing
------------
Yes! Please! Fork and pull request! If you experience any issues you don't know how to fix or have any
feture requests you're welcome to create an issue.

License
-------
This software is licensed under my "Do-whatever-the-fuck-you-want-but-don't-be-a-dick" license. This means you're
absolutely free to use, modify and redistribute this software. You can basically do whatever the fuck you want.
But don't be a dick and mention my name if you think I'm worth it.