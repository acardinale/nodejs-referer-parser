# referer-parser

referer-parser is a database for extracting marketing attribution data (such as search terms) from referer URLs, inspired by the [ua-parser][ua-parser] project (an equivalent library for user agent parsing).

The referer-parser project also contains multiple libraries for working with the referer-parser database in different languages.

referer-parser is a core component of [Snowplow][snowplow], the open-source web-scale analytics platform powered by Hadoop and Redshift.

_Note that we always use the original HTTP misspelling of 'referer' (and thus 'referal') in this project - never 'referrer'._

## Database

The latest database is always available on this URL:

https://s3-eu-west-1.amazonaws.com/snowplow-hosted-assets/third-party/referer-parser/referers-latest.yml

The database is updated at most once a month. Each new version of the database is also uploaded with a timestamp:

https://s3-eu-west-1.amazonaws.com/snowplow-hosted-assets/third-party/referer-parser/referers-YYYY-MM.yml

If there is an issue with the database necessitating a re-release within the month, the corresponding files will be overwritten.


# referer-parser node.js (JavaScript) library

This is the node.js (JavaScript) implementation of [referer-parser][referer-parser], the library for extracting search marketing data from referer _(sic)_ URLs.

The implementation uses the shared 'database' of known referers found in [`referers.yml`][referers-yml]

The Javascript version of referer-parser is maintained by [Martin Katrenik][mkatrenik].

## Installation

    $ npm install referer-parser

## Usage

Create a new instance of a Referer object by passing in the url you want to parse:

```js
var Referer = require('referer-parser')

referer_url = 'http://www.google.com/search?q=gateway+oracle+cards+denise+linn&hl=en&client=safari'

var r = new Referer(referer_url)
```

The `r` variable now holds a Referer instance.  The important attributes are:

```js
console.log(r.known)              // true
console.log(r.referer)            // 'Google'
console.log(r.medium)             // 'search'
console.log(r.search_parameter)   // 'q'
console.log(r.search_term)        // 'gateway oracle cards denise linn'
console.log(r.uri)                // result of require('url').parse(...)
```

Optionally, pass in the current URL as well, to handle internal referers

```js
var Referer = require('referer-parser')

var referer_url = 'http://www.snowplowanalytics.com/about/team'
var current_url = 'http://www.snowplowanalytics.com/account/profile'

var r = Referer(referer_url, current_url)
```

The attributes would be

```js
console.log(r.known)              // true
console.log(r.referer)            // null
console.log(r.medium)             // 'internal'
console.log(r.search_parameter)   // null
console.log(r.search_term)        // null
console.log(r.uri)                // result of require('url').parse(...)
```

## Copyright and license

The referer-parser node.js (JavaScript) library is copyright 2013 Martin Katrenik.

Licensed under the [Apache License, Version 2.0][license] (the "License");
you may not use this software except in compliance with the License.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[referer-parser]: https://github.com/snowplow/referer-parser
[referers-yml]: https://github.com/snowplow/referer-parser/blob/master/referers.yml

[mkatrenik]: https://github.com/mkatrenik

[license]: http://www.apache.org/licenses/LICENSE-2.0
