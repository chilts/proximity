# Proximity #

Proximity is a pluggable and configurable HTTP proxy/server.

## What does this mean? ##

It means that you can configure proximity on a per site basis and each site will define which strategy
should be employed.

e.g. if you want the entire site for ```www.chilts.org``` to redirect to ```chilts.org```, all you need to do is drop a
a proximity config file into ```/etc/proximity.d/www-chilts-org.ini```. It will look like this:

```
[www.chilts.org]
type=redirect
to=chilts.org
```

That's all you need to do for proximity to redirect every request on that site to the naked domain.

## Built in Strategies ##

You can build your own strategies for proximity, but the following is the set that comes with proximity.

### proxy ###

This strategy will proxy every request through to another server. For example if you are running a site called
```cssminifier.com``` on port 3000 on the same host, you would use the following strategy:

```
[cssminifier.com]
type=proxy
host=localhost
port=3000
```

### lb ###

(Not yet implemented.)

### redirect ###

A redirect site will look like:

```
[www.chilts.org]
type=redirect
to=chilts.org
```

### static ###

Proximity can also serve static sites out of the box. To do this use the following config:

```
[awssum.io]
type=static
dir=/path/to/awssum-io/htdocs
```

### not-found ###

This seems like a strange thing to for a domain but it comes in useful for proximity if it receives a request for a
site that is unknown. It was made into a strategy so that it could be re-used. All it does is return a ```404 - Not
found``` for every request to that domain.

```
[old.example.com]
type=not-found
```

### Your own Stragey ###

Or you can write your own strategy (I'd love a PR if it's a generic strategy). If it's something that could be useful
to everyone, let me know. :)

## Config File ##

Proximity reads the config file ```/etc/proximity.ini``` to get some settings. Currently the only setting read is
```port```. An example config file is:

```
port=80
```

## /etc/proximity.d/ ##

So that Proximity knows which sites to proxy, you should put files into the ```/etc/proximity.d/``` directory. An
example config file for a simple site might be:

e.g. ```/etc/proximity.d/chilts-org```:

```
[www.chilts.org]
type=redirect
to=chilts.org

[chilts.org]
type=proxy
host=localhost
port=3000
```

As you can see, all requests on the ```www.chilts.org``` subdomain will be redirected to the naked domain.

All requests on the naked domain will be proxied through to ```localhost:3000```.

It is up to your blog site to install a relevant file into ```/etc/proximity.d/``` so that proximity knows where to
proxy the site.

An example config you might use when locally developing a site could be:

```
[chilts.localhost]
type=proxy
host=localhost
port=3000
```

# Author #

Written by [Andrew Chilton](http://chilts.org/) - [Blog](http://chilts.org/blog/) -
[Twitter](https://twitter.com/andychilton).

# License #

* [Copyright 2013 Andrew Chilton.  All rights reserved.](http://chilts.mit-license.org/2013/)

(Ends)
