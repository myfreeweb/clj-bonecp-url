# STOP RIGHT THERE

[HikariCP](http://brettwooldridge.github.io/HikariCP/) is a new connection pool that's faster and smaller than BoneCP. There is [a Clojure wrapper](https://github.com/tomekw/hikari-cp) that doesn't seem to contain URL parsing code... You probably should take it from here but actually use it with HikariCP.

Current [semantic](http://semver.org/) version:

[![clojars version](https://clojars.org/clj-bonecp-url/latest-version.svg?raw=true)](https://clojars.org/clj-bonecp-url)

# clj-bonecp-url

A Clojure wrapper for [BoneCP](http://jolbox.com/), a JDBC connection pooling library.
Can parse URLs.
Works with Heroku Postgres, even remotely (ie. doesn't lose SSL parameters from the URLs).

## Usage

**NOTE:** you need to have an implementation of [slf4j](http://www.slf4j.org/) in your dependencies in addition to clj-bonecp-url itself.

If you don't care about logging:

```clojure
[org.slf4j/slf4j-nop "1.7.2"]
```

### Actual usage

Use it with [Korma](http://sqlkorma.com):

```clojure
(ns your.app
  (:use clj-bonecp-url.core
        korma.db))

(def datasource
  (datasource-from-url
    (or (System/getenv "DATABASE_URL")
        "postgres://user:pass@localhost:5432/db")))

(when (nil? @korma.db/_default)
  (korma.db/default-connection {:pool {:datasource datasource}}))

; defentity, etc.
```

or plain old java.jdbc:

```clojure
(ns your.app
  (:use clj-bonecp-url.core)
  (:require [clojure.java.jdbc :as jdbc]))

(def datasource
  (datasource-from-url
    (or (System/getenv "DATABASE_URL")
        "postgres://user:pass@localhost:5432/db")))

(jdbc/with-connection {:datasource datasource}
  (jdbc/with-query-results rows ["SELECT * FROM everything;"]
    (prn rows)))
```

## License

Copyright © 2013-2015 Greg V.  
Contains code from [clj-bonecp](https://github.com/opiskelijarekisteri-devel/clj-bonecp) and [clj-dbcp](https://github.com/kumarshantanu/clj-dbcp).

Distributed under the Eclipse Public License, the same as Clojure.
