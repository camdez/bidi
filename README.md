# bidi

> "bidi bidi bidi" -- Twiki, in probably every episode of
  [Buck Rogers in the 25th Century](http://en.wikipedia.org/wiki/Buck_Rogers_in_the_25th_Century_%28TV_series%29)

In the grand tradition of Clojure libraries we begin with an irrelevant
quote.

Bi-directional URI routing between handlers and routes. Like
[Compojure](https://github.com/weavejester/compojure), but when you want
to go both ways. For example, many routing libraries can route a URI to
a request handler, but only a fraction of these (for example,
[Pedestal](http://pedestal.io),
[gudu](https://github.com/thatismatt/gudu)) can generate a URI from a
handler. If you are serving REST resources, you should be
[providing links](http://en.wikipedia.org/wiki/HATEOAS) to others
resources, and without full support for generating URIs from handlers
your code will become coupled with your routing. In short, hard-coded
URIs will eventually break.

In bidi, routes are data structures, there are no macros here.

The logic for matching routes is separated from the responsibility for
handling requests. This is an important
[architectural principle](http://www.infoq.com/presentations/Simple-Made-Easy). So
you can match on things that aren't necessarily handlers, like keywords
which you can use to lookup your handlers, or whatever you want to
do. Separation of concerns and all that.

## Usage

```clojure
(require '[bidi.bidi :refer (match-route)])

(match-route ["/blog" [["/foo" 'foo]
                       [["/bar" "/articles/" :artid "/index.html"] 'bar]]]
             {:path "/blog/bar/articles/123/index.html"})
```

returns

```clojure
{:handler 'bar, :params {:artid "123"}, :path "/bar/articles/123/index.html"}
```

You can also go the other way

```clojure
(require '[bidi.bidi :refer (unmatch-route)])

(unmatch-route ["/blog"
                [["/index.html" 'blog-index]
                 [["/article/" :id ".html"] 'blog-article-handler]
                 [["/archive/" :id "/old.html"] 'foo]]]
               {:handler 'blog-article-handler :params {:id 1239}})
```

returns

```clojure
"/blog/article/1239.html"
```

Nice.

## Status

This is pre-alpha, and I know there's a few bugs, so don't use until I remove this message.

## License

Copyright © 2013, JUXT LTD. All Rights Reserved.

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.