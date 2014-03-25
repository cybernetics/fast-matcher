# fast-matcher

Most autocomplete implementations I've seen do full-list scanning and simple (infix) text matching.

The standard performance optimizations seem to be complex: indexing, caching, *tries*, etc.

This library is a simpler and **much faster** "back end" for autocompletes. It isn't as
full-featured, which means it can make some assumptions for really big perf wins:

1. It only does prefix matching.

In my experience this actually makes perfect sense in a lot of cases. For example suppose your user
is searching for a company name; they aren't going to type "o" or "e" to search for *Home Depot*. Or
say they're using a dictionary; they aren't going to type "n" to search for *anagram*.

See for yourself how fast this is in practice: the [demo](http://danieltao.com/fast-matcher) uses
this library to provide autocomplete results from [WordNet](http://wordnet.princeton.edu/), with
close to 150,000 words. Results are instantaneous.

## Usage

```javascript
var FastMatcher = require('fast-matcher');

var matcher = new FastMatcher([{x:'foo'},{x:'bar'},{x:'baz'}], {
  /* options */

  // the property to base matches on (omit for a simple list of strings)
  selector: 'x',

  // duh, what do you think this does?
  caseInsensitive: true,

  // how many matches to find at a time
  limit: 25
});

matcher.getMatches('ba');
// => [{x:'bar'},{x:'baz'}]
```
