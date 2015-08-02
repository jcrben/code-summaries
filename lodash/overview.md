<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Size in lines of code](#size-in-lines-of-code)
  - [lodash.core.js](#lodashcorejs)
  - [lodash.js](#lodashjs)
- [Complexity analysis](#complexity-analysis)
- [Initialization and first impressions](#initialization-and-first-impressions)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

As of July 30, 2015, based on [commit 1ca0ea8b80feed37b75df6310e82893c89bf22e9](https://github.com/lodash/lodash/tree/1ca0ea8b80feed37b75df6310e82893c89bf22e9), which is near the 3.10.10 release tag. All links to source should be as of this date as well.

#### Size in lines of code
Size of the core code using https://github.com/flosse/sloc. Lodash is a single file program. Historically there has been a `lodash.js` and `lodash.src.js` representing slightly different programs. As of the above commit, however, maintainer John Dalton had just added `lodash.core.js` with fewer functions and about a 1/3 the size in lines of code. 

##### lodash.core.js
```
┌──────────┬────────┬─────────┐
│ Physical │ Source │ Comment │
├──────────┼────────┼─────────┤
│ 3491     │ 1092   │ 2163    │
└──────────┴────────┴─────────┘
```
##### lodash.js
```
┌──────────┬────────┬─────────┐
│ Physical │ Source │ Comment │
├──────────┼────────┼─────────┤
│ 11783    │ 4222   │ 6825    │
└──────────┴────────┴─────────┘
```
#### Complexity analysis

Running this through codeclimate.com generated a very low score due to high complexity and potential duplication. I don't mean to endorse these results as meaning that the code should be refactored (those involved, please don't take it personally).

| Rating | Name                  | LOC  | Duplication | Churn | Issues |
|--------|-----------------------|------|-------------|-------|--------|
| F      | lodash.core.js        | 1080 | 2349        | 1     | 91     |
| F      | lodash.js             | 4218 | 2655        | 43    | 109    |
| A      | perf/asset/perf-ui.js | 104  | 0           | 1     | 4      |
| F      | perf/perf.js          | 1788 | 1519        | 3     | 101    |



#### Initialization and first impressions
I will use the lodash.core.js file even though it is quite new as it should demonstrate the fundamentals.

My motivation for studying this was to add a method similar to pullAt() as a method on the lodash wrapper object which instead of mutating the aray returned a copy of the array with value at the index spliced out. I'm not aware of an existing function out there for this: one could perhaps call it sliceAt(). It would be used in place of `var arr = slice(0, i) + slice(i + 1)`.

As I studied lodash, I found that figuring out how it was built up was a bit more complex than expected. The mixin method is called ([source L3389](https://github.com/lodash/lodash/blob/1ca0ea8b80feed37b75df6310e82893c89bf22e9/lodash.core.js#L3389)) to add functions to the lodash prototype. From there, the following functions are called: 

* mixin calls baseForOwn() ([source L3428](https://github.com/lodash/lodash/blob/1ca0ea8b80feed37b75df6310e82893c89bf22e9/lodash.core.js#L3428)), 
* baseForOwn() calls baseFor() ([source L606](https://github.com/lodash/lodash/blob/1ca0ea8b80feed37b75df6310e82893c89bf22e9/lodash.core.js#L606)), 
* baseFor() is created by createBaseFor() ([L596](https://github.com/lodash/lodash/blob/1ca0ea8b80feed37b75df6310e82893c89bf22e9/lodash.core.js#L596))
* createBaseFor() deserves further study as it returns "a base function for methods like _.forIn"
